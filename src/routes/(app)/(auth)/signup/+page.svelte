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
					shouldCreateUser: true 
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
	let errorMessage: string | null = null;
	let isResending = false;

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
		errorMessage = null;

		const { data, error } = await supabase.auth.verifyOtp({
			email,
			token: otpCode,
			type: 'email'
		});

		isFormLoading = false;

		if (error) {
			errorMessage = error.message;
			toast.error('OTP verification failed', { description: error.message });
		} else {
			errorMessage = null;
			toast.success('Logged in successfully');
			// redirect, update session, etc.
		}
	}

	async function resendOtp() {
		isResending = true;
		errorMessage = null;
		const { error } = await supabase.auth.signInWithOtp({
			email,
			options: {
				shouldCreateUser: true
			}
		});
		isResending = false;
		if (error) {
			errorMessage = error.message;
			toast.error('Resend OTP failed', { description: error.message });
		} else {
			toast.success('OTP resent', { description: 'Check your email for a new OTP.' });
		}
	}
</script>

<svelte:head>
	<title>Sign Up | Lipsync</title>
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
			<p class="text-sm text-muted-foreground">Signup with an account</p>
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
						Sign up with Email</Form.Button
					>
				</form>
			{:else if step === 2}
				<form on:submit|preventDefault={verifyOtp} class="max-w-md mx-auto p-8 rounded-xl shadow-md space-y-6">
					<div class="relative">
						<label for="otp-input" class="sr-only">Enter OTP</label>
						<input
						id="otp-input"
						bind:value={otpCode}
						type="text"
						inputmode="numeric"
						pattern="[0-9]*"
						maxlength="6"
						placeholder="6-digit OTP"
						required
						class="w-full px-4 py-3 text-lg tracking-[0.5em] text-center border-2 border-gray-900 rounded-lg focus:ring-gray-900 focus:border-gray-900 transition-all disabled:bg-gray-900"
						disabled={isFormLoading}
						/>
						{#if isFormLoading}
						<div class="absolute right-3 top-1/2 -translate-y-1/2">
							<svg class="w-6 h-6 animate-spin text-gray-900" viewBox="0 0 24 24">
							<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
							<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
							</svg>
						</div>
						{/if}
					</div>
					
					<button
						type="submit"
						disabled={isFormLoading}
						class="w-full py-3 px-4 bg-gray-900 hover:bg-gray-900 text-white font-semibold rounded-lg transition-colors disabled:bg-gray-900 disabled:cursor-not-allowed"
					>
						{#if isFormLoading}
							<span class="flex items-center justify-center gap-2">
								<svg class="w-4 h-4 animate-spin" viewBox="0 0 24 24">
									<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
									<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
								</svg>
								Verifying...
							</span>
						{:else}
							Verify OTP
						{/if}
					</button>
					
					{#if errorMessage}
						<div role="alert" class="flex items-center gap-2 p-3 text-sm text-red-700 bg-red-100 rounded-lg">
						<svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 flex-shrink-0" viewBox="0 0 20 20" fill="currentColor">
							<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
						</svg>
						{errorMessage}
						</div>
					{/if}
					
					<div class="text-sm text-center text-gray-500">
						Didn't receive code?{' '}
						<button
						type="button"
						on:click={resendOtp}
						disabled={isResending}
						class="font-semibold text-blue-500 hover:text-blue-700 hover:underline disabled:text-gray-400 disabled:cursor-not-allowed"
						>
						{isResending ? 'Sending...' : 'Resend OTP'}
						</button>
					</div>
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
			<a href="/signin" class="hover:text-brand underline underline-offset-4">
				Already have an account? Sign in
			</a>
		</p>
	</div>
</div>
