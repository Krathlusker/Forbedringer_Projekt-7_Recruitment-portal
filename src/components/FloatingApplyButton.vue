<template>
	<el-button size="large" class="floating-apply-button btn-red" @click="$emit('click')">
		ANSØG NU
	</el-button>
</template>

<script setup lang="ts">

defineEmits<{
	click: []
}>()
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
	@include tablet {
		position: fixed;
		top: 70%;
		right: 0;
		writing-mode: vertical-rl;
		height: auto !important;
		z-index: $z-index-floating-button;
		border-radius: $border-radius-sm 0 0 $border-radius-sm !important;
		padding: $spacing-md calc($spacing-md / 2) $spacing-md calc($spacing-md / 2) !important;
		transition: padding $transition-duration $transition-ease-smooth,
			background-color $transition-duration $transition-ease,
			color $transition-duration $transition-ease,
			box-shadow $transition-duration $transition-ease !important;

		// Rotér teksten 180° så den læses nedefra-og-op (som originalt)
		:deep(> span) {
			display: inline-block;
			transform: rotate(180deg);
		}

		&:hover {
			padding: $spacing-md $spacing-md $spacing-md calc($spacing-md / 2) !important;
			box-shadow: 0px 8px 20px rgba(var(--el-color-primary-rgb), 0.4) !important;
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
		padding: $spacing-md calc($spacing-md / 2) $spacing-md calc($spacing-md / 2) !important;
		transition: padding $transition-duration $transition-ease-smooth,
			background-color $transition-duration $transition-ease,
			color $transition-duration $transition-ease,
			box-shadow $transition-duration $transition-ease !important;

		:deep(> span) {
			display: inline-block;
			transform: rotate(180deg);
		}

		&:hover {
			padding: $spacing-md $spacing-md $spacing-md calc($spacing-md / 2) !important;
			box-shadow: 0px 8px 20px rgba(var(--el-color-primary-rgb), 0.4) !important;
		}
	}
}
</style>
