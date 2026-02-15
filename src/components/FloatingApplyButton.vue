<template>
	<el-button
		size="large"
		class="floating-apply-button btn-red"
		:class="{ 'is-dragging': isDragging, 'is-nudging': isNudging }"
		:style="dragStyle"
		@click="handleClick"
		@touchstart.passive="onTouchStart"
		@touchmove.passive="onTouchMove"
		@touchend="onTouchEnd"
	>
		ANSØG NU
	</el-button>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const emit = defineEmits<{
	click: []
}>()

// ── Drag-to-apply (mobil/tablet touch) ──────────────────
// Brugeren trækker knappen mod venstre → padding øges (som hover).
// Når fingeren slippes snapper padding tilbage, og modalen åbnes
// hvis tærsklen er nået.
//
// Max extra padding = $spacing-md / 2 = 9px (matcher hover-state)
// DRAG_RANGE = antal px fingeren skal trækkes for at nå max padding
const MAX_EXTRA_PAD = 14
const DRAG_RANGE = 25
const TRIGGER_RATIO = 0.5 // åbn modal hvis ≥50 % af maks

const dragPad = ref(0)
const isDragging = ref(false)
const wasDragged = ref(false)
let startX = 0

// Sætter CSS custom properties for padding + farve-interpolation under drag
// SCSS bruger --drag-ratio med color-mix() til at blende farverne
const dragStyle = computed(() => {
	if (dragPad.value === 0) return undefined
	const ratio = Math.round((dragPad.value / MAX_EXTRA_PAD) * 100)
	return {
		'--drag-pad-extra': `${dragPad.value}px`,
		'--drag-ratio': `${ratio}%`,
	} as Record<string, string>
})

function onTouchStart(e: TouchEvent) {
	startX = e.touches[0].clientX
	isDragging.value = true
	wasDragged.value = false
	dragPad.value = 0
}

function onTouchMove(e: TouchEvent) {
	if (!isDragging.value) return
	// positive diff = finger bevæger sig mod venstre
	const diff = startX - e.touches[0].clientX
	if (diff > 5) wasDragged.value = true
	// Lineær mapping: 0-DRAG_RANGE px finger-bevægelse → 0-MAX_EXTRA_PAD px padding
	const ratio = Math.max(0, Math.min(1, diff / DRAG_RANGE))
	dragPad.value = ratio * MAX_EXTRA_PAD
}

function onTouchEnd() {
	if (!isDragging.value) return
	const shouldTrigger = dragPad.value >= MAX_EXTRA_PAD * TRIGGER_RATIO
	// Fjern is-dragging → CSS padding-transition genaktiveres
	isDragging.value = false
	// Næste frame: nulstil padding → CSS animerer snap-back
	requestAnimationFrame(() => {
		dragPad.value = 0
		if (shouldTrigger) {
			// Vent på snap-back animationen (padding transition ~200ms)
			setTimeout(() => emit('click'), 250)
		}
	})
}

function handleClick() {
	// Undgå at click fyrer efter et drag
	if (wasDragged.value) {
		wasDragged.value = false
		return
	}
	emit('click')
}

// ── Hint-nudge ──────────────────────────────────────────
// Kort CSS-animation der "popper" knappen ud 50 %
// for at minde brugeren om at den kan trækkes.
const isNudging = ref(false)
let nudgeTimeout: ReturnType<typeof setTimeout> | null = null
let nudgeTimer: ReturnType<typeof setInterval> | null = null

function playNudge() {
	if (isDragging.value) return
	// Sæt padding til maks → CSS transition animerer ud
	isNudging.value = true
	dragPad.value = MAX_EXTRA_PAD
	// Efter 150ms: nulstil → CSS transition animerer tilbage
	setTimeout(() => {
		dragPad.value = 0
		setTimeout(() => { isNudging.value = false }, 150)
	}, 150)
}

onMounted(() => {
	// Første nudge efter 3 sek, derefter hvert 30. sek
	nudgeTimeout = setTimeout(() => {
		playNudge()
		nudgeTimer = setInterval(playNudge, 30_000)
	}, 3000)
})

onUnmounted(() => {
	if (nudgeTimeout) clearTimeout(nudgeTimeout)
	if (nudgeTimer) clearInterval(nudgeTimer)
})
</script>

<style lang="scss" scoped>
.floating-apply-button {
	// Desktop: normal button in header flow
	@include desktop {
		position: static;
		writing-mode: horizontal-tb;
		transform: none;
		transform-origin: right center;
		z-index: auto;
		transition: background-color $transition-duration $transition-ease,
			color $transition-duration $transition-ease,
			box-shadow $transition-duration $transition-ease,
			transform $transition-duration-slow $transition-ease-smooth !important;

		&:hover {
			box-shadow: 0px 8px 20px rgba(var(--el-color-primary-rgb), 0.4) !important;
		}
	}

	// Mobile & tablet: side-tab fastgjort til højre kant
	// Bruger writing-mode i stedet for transform: rotate() —
	// det gør padding-animation triviel (ingen rotation at animere)
	//
	// --drag-pad-extra (CSS custom property, sat via JS):
	//   Øger right-padding under drag.  0px = normal, 9px = maks (= hover).
	@include tablet {
		position: fixed;
		top: 70%;
		right: 0;
		writing-mode: vertical-rl;
		height: auto !important;
		z-index: $z-index-floating-button;
		border-radius: $border-radius-sm 0 0 $border-radius-sm !important;
		padding: $spacing-md calc($spacing-md / 2 + var(--drag-pad-extra, 0px)) $spacing-md calc($spacing-md / 2) !important;
		transition: padding $transition-duration $transition-ease-smooth,
			background-color $transition-duration $transition-ease,
			color $transition-duration $transition-ease,
			box-shadow $transition-duration $transition-ease !important;

		// Rotér teksten 180° så den læses nedefra-og-op (som originalt)
		:deep(> span) {
			display: inline-block;
			transform: rotate(180deg);
		}

		@media (hover: hover) {
			&:hover {
				padding: $spacing-md $spacing-md $spacing-md calc($spacing-md / 2) !important;
				box-shadow: 0px 8px 20px rgba(var(--el-color-primary-rgb), 0.4) !important;
			}
		}
	}

	@include mobile {
		position: fixed;
		top: 70%;
		right: 0;
		writing-mode: vertical-rl;
		height: auto !important;
		z-index: $z-index-floating-button;
		border-radius: $border-radius-sm 0 0 $border-radius-sm !important;
		padding: $spacing-md calc($spacing-md / 2 + var(--drag-pad-extra, 0px)) $spacing-md calc($spacing-md / 2) !important;
		transition: padding $transition-duration $transition-ease-smooth,
			background-color $transition-duration $transition-ease,
			color $transition-duration $transition-ease,
			box-shadow $transition-duration $transition-ease !important;

		:deep(> span) {
			display: inline-block;
			transform: rotate(180deg);
		}

		@media (hover: hover) {
			&:hover {
				padding: $spacing-md $spacing-md $spacing-md calc($spacing-md / 2) !important;
				box-shadow: 0px 8px 20px rgba(var(--el-color-primary-rgb), 0.4) !important;
			}
		}
	}

	// Under aktivt drag: ingen transition + brug color-mix() til farve-interpolation
	&.is-dragging {
		transition: none !important;
		background-color: color-mix(in srgb, var(--el-color-danger), var(--el-bg-color) var(--drag-ratio, 0%)) !important;
		color: color-mix(in srgb, var(--el-bg-color), var(--el-color-danger) var(--drag-ratio, 0%)) !important;
	}

	// Hint-nudge: kort padding-puls via JS (bruger --drag-pad-extra)
	&.is-nudging:not(.is-dragging) {
		// reserveret til evt. fremtidig styling
	}
}
</style>
