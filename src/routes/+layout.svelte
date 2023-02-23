<script lang="ts">
	import { supabase } from '$lib/supabaseClient';
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';
	import './styles.css';

	onMount(() => {
		// get subscription to supabase database
		const {
			data: { subscription }
		} = supabase.auth.onAuthStateChange(() => {
			// invalidate supabase auth when auth state changes
			invalidate('supabase:auth');
		});

		// unsubscribe when app unmounts
		return () => {
			subscription.unsubscribe();
		};
	});
</script>

<div class="container" style:padding={'50px 0 100px 0'}>
	<!-- need a slot in the layout for pages to be placed in -->
	<slot />
</div>
