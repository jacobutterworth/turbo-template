<script lang="ts">
	interface $$Props {
		onClick: () => void;
		class?: string;
	}

	const { onClick, class: className = '' } = $props();
</script>

<div
	class={`${className}`}
	onclick={onClick}
	role="button"
	tabindex="0"
	aria-roledescription="button"
	onkeydown={(e) => {
		if (e.key === 'Enter' || e.key === ' ') {
			onClick();
		}
	}}
>
	<slot />
</div>
