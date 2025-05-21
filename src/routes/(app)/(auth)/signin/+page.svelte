<script lang="ts">
	import GitHubSvg from '$lib/imgs/github-dark.svg';
	import Button from '$lib/components/ui/button/button.svelte';
	import { ChevronLeftIcon } from 'lucide-svelte';

	import * as Form from '$lib/components/ui/form';
	import { Input } from '$lib/components/ui/input';
	import { formSchema, type FormSchema } from '$lib/schema/schema';
	import { type SuperValidated, type Infer, superForm } from 'sveltekit-superforms';
	import { zodClient } from 'sveltekit-superforms/adapters';
	import { Loader } from 'lucide-svelte';
	// import { enhance } from '$app/forms';
	import { toast } from 'svelte-sonner';
	import { supabase } from '$lib/supabaseClient';

	let step = 1;
	let email = '';
	let otpCode = '';


	export let data;
	let dataForm: SuperValidated<Infer<FormSchema>> = data.form;
	let form = superForm(dataForm, {
		validators: zodClient(formSchema),
		onSubmit: async ({ formData }) => {
			isFormLoading = true;

			const formEmail = formData.get('email');
			email = formEmail as string;
			const { error } = await supabase.auth.signInWithOtp({
				email: email,
				options: {
					shouldCreateUser: false 
				}
			});

			if (error) {
				toast.error('Sign in failed', {
					description: error.message
				});
			} else {
				step = 2;
				toast.success('Check your email', {
					description: 'We sent you an OTP link. Check your inbox (and spam).'
				});
			}

			isFormLoading = false;
		},
		onUpdate: ({ result }) => {
			isFormLoading = false;
			if (result.status === 200) {
				toast.success('Check your email', {
					description: 'We have sent you a login link. Be sure to check your spam too.'
				});
			} else {
				toast.error('Something went wrong', {
					description: 'Your sign in request failed. Please try again.'
				});
			}
		}
	});

	const { form: formData, enhance } = form;

	let loading = false;
	let isFormLoading = false;
	const githubSignIn = async () => {
		loading = true;
		const { error } = await supabase.auth.signInWithOAuth({ provider: 'github' });
		if (error) {
			toast.error('GitHub sign-in failed', { description: error.message });
		}
		loading = false;
	};

	
	async function verifyOtp() {
		isFormLoading = true;

		const { data, error } = await supabase.auth.verifyOtp({
			email,
			token: otpCode,
			type: 'email'
		});

		isFormLoading = false;

		if (error) {
			toast.error('OTP verification failed', { description: error.message });
		} else {
			toast.success('Logged in successfully');
			// redirect, update session, etc.
		}
	}

</script>

<svelte:head>
	<title>Sign In | Lipsync</title>
	<meta name="description" content="Sign In for Svee UI" />
</svelte:head>

<div class="container flex h-screen w-screen flex-col items-center justify-center">
	<Button variant="ghost" href="/" class="absolute left-4 top-4 md:left-8 md:top-8">
		<ChevronLeftIcon class="mr-2 size-4" />
		Back
	</Button>





	<div class="mx-auto flex w-full flex-col justify-center gap-6 sm:w-[350px]">
		<div class="flex flex-col gap-2 text-center">
			<!-- {/* <Icons.logo class="mx-auto h-6 w-6" /> */} -->
			<h1 class="text-2xl font-semibold tracking-tight">Welcome back</h1>
			<p class="text-sm text-muted-foreground">Login to your account</p>
		</div>
		{#if step === 1}
		<!-- Form -->
				<form method="POST" use:enhance>
					<Form.Field {form} name="email" class="mb-4">
						<Form.Control let:attrs>
							<Input placeholder="name@example.com" {...attrs} bind:value={$formData.email} />
						</Form.Control>
						<!-- <Form.Description>This is your email address.</Form.Description> -->
						<Form.FieldErrors />
					</Form.Field>
					<Form.Button size="sm" class="w-full" disabled={isFormLoading}>
						{#if isFormLoading}
							<Loader class="mr-2 size-4 animate-spin" />
						{/if}
						Sign In with Email</Form.Button
					>
				</form>
			{:else if step === 2}
				<form on:submit|preventDefault={verifyOtp}>
					<input bind:value={otpCode} type="text" placeholder="Enter OTP" required />
					<Button type="submit" disabled={isFormLoading}>Verify OTP</Button>
				</form>
		{/if}
		<!-- Separator -->
		<div class="relative">
			<div class="absolute inset-0 flex items-center">
				<span class="w-full border-t" />
			</div>
			<div class="relative flex justify-center text-xs uppercase">
				<span class="bg-background px-2 text-muted-foreground"> Or continue with </span>
			</div>
		</div>
		<Button on:click={githubSignIn} variant="outline" disabled={loading}>
			{#if loading}
				<Loader class="mr-2 size-4 animate-spin" />
			{:else}
				<img src={GitHubSvg} alt="github" class="mr-2 size-4" />
			{/if}
			Github</Button
		>
		<p class="px-8 text-center text-sm text-muted-foreground">
			<a href="/signup" class="hover:text-brand underline underline-offset-4">
				Don&apos;t have an account? Sign Up
			</a>
		</p>
	</div>
</div>
