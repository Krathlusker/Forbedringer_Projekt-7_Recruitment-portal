<template>
	<div
		ref="containerRef"
		class="video-player-v2"
		:class="{
			'video-player-v2--contain': objectFit === 'contain',
			'video-player-v2--rounded': rounded
		}"
		:style="{ '--video-max-height': `${maxHeight}px` }"
	>
		<!-- Title overlay (above video) - visible in normal mode -->
		<div
			v-if="title"
			class="video-player-v2__title-wrapper"
		>
			<h2 class="video-player-v2__title">{{ title }}</h2>
			<el-text v-if="subtitle" class="video-player-v2__subtitle">{{ subtitle }}</el-text>
		</div>

		<!-- Video.js Player -->
		<VideoPlayer
			class="video-player-v2__player video-js vjs-big-play-centered"
			:src="src"
			:poster="computedPoster"
			:loop="loop"
			:muted="muted"
			:autoplay="autoplay"
			:controls="true"
			:playsinline="true"
			:volume="initialVolume"
			:preload="preload"
			:options="videoOptions"
			@mounted="onPlayerMounted"
			@play="onPlay"
			@pause="onPause"
			@ended="onEnded"
			@timeupdate="onTimeUpdate"
		>
			<!-- Title overlay INSIDE video.js for fullscreen mode -->
			<template #default>
				<div
					v-if="title"
					class="video-player-v2__title-wrapper video-player-v2__title-wrapper--fullscreen"
				>
					<h2 class="video-player-v2__title">{{ title }}</h2>
					<el-text v-if="subtitle" class="video-player-v2__subtitle">{{ subtitle }}</el-text>
				</div>
				<slot name="overlay" />
			</template>
		</VideoPlayer>
	</div>
</template>

<script setup lang="ts">
import { VideoPlayer } from '@videojs-player/vue'
import 'video.js/dist/video-js.css'
import type videojs from 'video.js'
import { getPosterPath } from '@/utils/mediaPreloader'

// Types
type Player = ReturnType<typeof videojs>

// Video.js player state (from @videojs-player/vue)
interface VideoPlayerState {
	playing: boolean
	currentTime: number
	duration: number
}

// Props
interface Props {
	src: string
	poster?: string
	title?: string
	subtitle?: string
	autoplay?: boolean
	loop?: boolean
	muted?: boolean
	objectFit?: 'cover' | 'contain'
	initialVolume?: number
	rounded?: boolean
	maxHeight?: number // Max height in pixels
	preload?: 'auto' | 'metadata' | 'none' // Controls when browser downloads video
}

const props = withDefaults(defineProps<Props>(), {
	poster: '',
	title: '',
	subtitle: '',
	autoplay: true,
	loop: true,
	muted: true,
	objectFit: 'cover',
	initialVolume: 0.6,
	rounded: false,
	maxHeight: 600,
	preload: 'auto' // Default: auto for autoplay videos
})

// Auto-generate poster path from video src if not provided
const computedPoster = computed(() => {
	if (props.poster) return props.poster
	return getPosterPath(props.src)
})

// Emits
const emit = defineEmits<{
	play: []
	pause: []
	ended: []
	timeupdate: [currentTime: number, duration: number]
	mounted: [player: Player]
}>()

// State
const player = ref<Player | null>(null)
const containerRef = ref<HTMLElement | null>(null)
const hasStarted = ref(false)

// Video.js options - includes preload setting to ensure it's respected
const videoOptions = computed(() => ({
	// Preload setting - must be in options for Video.js to respect it
	preload: props.preload,
	// Responsive behavior
	responsive: true,
	// Language
	language: 'da',
	// Playback speed options
	playbackRates: [0.5, 0.75, 1, 1.25, 1.5, 2],
	// Control bar config
	controlBar: {
		volumePanel: {
			inline: false,
			vertical: true
		},
		children: [
			'playToggle',
			'volumePanel',
			'currentTimeDisplay',
			'timeDivider',
			'durationDisplay',
			'progressControl',
			'playbackRateMenuButton',
			'fullscreenToggle'
		]
	},
	// User activity timeout (hide controls)
	inactivityTimeout: 2000
}))

// Touch scrub tooltip for iOS Safari.
// Video.js doesn't create .vjs-mouse-display on touch devices,
// so we create our own tooltip element and position it manually.
// We use capture phase to ensure our listener fires before Video.js handlers.
const setupTouchTooltip = (p: Player) => {
	const progressControl = p.el().querySelector('.vjs-progress-control') as HTMLElement | null
	const progressHolder = p.el().querySelector('.vjs-progress-holder') as HTMLElement | null
	if (!progressControl || !progressHolder) return

	// Only activate on touch devices
	if (!('ontouchstart' in window)) return

	// Create custom tooltip element inside the holder (for positioning context)
	const tip = document.createElement('div')
	tip.className = 'vjs-touch-tooltip'
	tip.setAttribute('aria-hidden', 'true')
	tip.style.display = 'none'
	progressHolder.appendChild(tip)

	const formatTime = (seconds: number): string => {
		const m = Math.floor(seconds / 60)
		const s = Math.floor(seconds % 60)
		return `${m}:${s.toString().padStart(2, '0')}`
	}

	const show = (touch: Touch) => {
		const rect = progressHolder.getBoundingClientRect()
		const fraction = Math.max(0, Math.min(1, (touch.clientX - rect.left) / rect.width))
		const time = fraction * (p.duration() || 0)
		tip.textContent = formatTime(time)
		tip.style.left = `${fraction * 100}%`
		tip.style.display = ''
	}

	const hide = () => {
		tip.style.display = 'none'
	}

	// Use capture phase to fire before Video.js internal handlers
	progressControl.addEventListener('touchstart', (e) => {
		if (e.touches.length === 1) show(e.touches[0])
	}, { passive: true, capture: true })

	progressControl.addEventListener('touchmove', (e) => {
		if (e.touches.length === 1) show(e.touches[0])
	}, { passive: true, capture: true })

	progressControl.addEventListener('touchend', hide, { passive: true, capture: true })
	progressControl.addEventListener('touchcancel', hide, { passive: true, capture: true })
}

// Event handlers
const onPlayerMounted = (payload: { video: HTMLVideoElement; player: Player; state: VideoPlayerState }) => {
	player.value = payload.player

	// Listen for useractive/userinactive to toggle container class (only after video has started)
	payload.player.on('useractive', () => {
		if (hasStarted.value) {
			containerRef.value?.classList.add('video-player-v2--controls-visible')
		}
	})
	payload.player.on('userinactive', () => {
		containerRef.value?.classList.remove('video-player-v2--controls-visible')
	})

	// Touch-to-mouse forwarding on progress bar — makes tooltip visible on iOS Safari
	setupTouchTooltip(payload.player)

	emit('mounted', payload.player)
}

const onPlay = () => {
	// Mark as started on first play - now title can move/fade with controls
	if (!hasStarted.value) {
		hasStarted.value = true
		containerRef.value?.classList.add('video-player-v2--has-started')
		// Set controls visible immediately since user just clicked
		if (player.value?.userActive()) {
			containerRef.value?.classList.add('video-player-v2--controls-visible')
		}
	}
	emit('play')
}
const onPause = () => emit('pause')
const onEnded = () => emit('ended')

const onTimeUpdate = () => {
	const p = player.value
	if (p) {
		emit('timeupdate', p.currentTime() || 0, p.duration() || 0)
	}
}

// Expose player instance for parent control
defineExpose({
	player,
	play: () => player.value?.play(),
	pause: () => player.value?.pause(),
	togglePlay: () => {
		if (player.value?.paused()) {
			player.value?.play()
		} else {
			player.value?.pause()
		}
	},
	mute: () => player.value?.muted(true),
	unmute: () => player.value?.muted(false),
	setVolume: (vol: number) => player.value?.volume(vol),
	seek: (time: number) => player.value?.currentTime(time),
	enterFullscreen: () => player.value?.requestFullscreen(),
	exitFullscreen: () => player.value?.exitFullscreen()
})
</script>

<style lang="scss" scoped>
// =============================================================================
// VIDEO PLAYER V2 - SCOPED STYLES
// =============================================================================
// Critical max-height styles that need scoped specificity to work.
// Uses CSS custom property --video-max-height set via inline style on root.

.video-player-v2 {
	position: relative;
	width: 100%;
	max-height: var(--video-max-height);
	background-color: #2d2d2d;
	overflow: hidden;

	// Ensure CSS custom property is inherited by ALL descendants
	*, *::before, *::after {
		--video-max-height: inherit;
	}
}

// Player container - Video.js wrapper
.video-player-v2__player {
	width: 100%;
	max-height: var(--video-max-height);
	aspect-ratio: 1 / 1; // Fallback until video loads

	// Video.js container - target the dynamic dimensions class
	// Video.js generates classes like vjs_video_123-dimensions
	:deep([class*="-dimensions"]) {
		width: 100% !important;
		height: auto !important;
		max-height: var(--video-max-height) !important;
	}

	// Also target .video-js directly
	:deep(.video-js) {
		font-family: helvetica-neue-lt-pro, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Ubuntu, Cantarell, 'Fira Sans', Arial, sans-serif;
		max-height: var(--video-max-height);
		aspect-ratio: 1 / 1;
	}

	// Video element itself
	:deep(.vjs-tech) {
		width: 100%;
		height: auto;
		max-height: var(--video-max-height);
		object-fit: contain;
		background-color: var(--el-color-primary);
	}

	// Poster - must be constrained within video-js
	:deep(.vjs-poster) {
		background-size: cover;
		max-height: var(--video-max-height);
	}
}
</style>
