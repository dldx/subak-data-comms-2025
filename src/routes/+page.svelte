<script lang="ts">
	import { Presentation, Slide, Transition, Action, getPresentation } from '@animotion/core'
	import { tween } from '@animotion/motion'
	import RagPipeline from '$lib/RagPipeline.svelte'
	import * as Tabs from '$lib/components/ui/tabs/index.js'
	import { Button } from '$lib/components/ui/button/index.js'
	import ChevronRightIcon from '@lucide/svelte/icons/chevron-right'
	import ChevronLeftIcon from '@lucide/svelte/icons/chevron-left'

	let audienceSegments = tween({ x: -400, opacity: 0 })
	let dataPoints = tween({ scale: 0, opacity: 0 })
	let impactMetrics = tween({ y: 100, opacity: 0 })
	let messagePersonalization = tween({ x: 0, opacity: 1 })

	let currentOutput = $state('nature-scorecard')
	let outputs = ['nature-scorecard', 'country-profiles', 'scrollytelling', 'data-portal']
	let currentIndex = $state(0)

	const outputUrls: Record<string, Record<string, string>> = {
		'nature-scorecard': {
			'nature-scorecard': 'https://planet-tracker.org/nature-scorecard/'
		},
		'data-portal': {
			'data-portal': 'https://data.carbontracker.org'
		},
		'country-profiles': {
			'country-profiles': 'https://countryprofiles.carbontracker.org'
		},
		scrollytelling: {
			'women-in-parliament': 'https://pudding.cool/2018/07/women-in-parliament',
			'japan-carbon-pricing': 'https://carbon-tracker-initiative.github.io/japan-interactive-2022/'
		}
	}
	let customAudiences = $state<string[]>([])
	let newAudienceInput = $state('')
	let hideCustomAudiences = $state(false)
	// Add data outputs state variables
	let customDataOutputs = $state<string[]>([])
	let newDataOutputInput = $state('')
	let hideCustomDataOutputs = $state(false)
	let presentation = getPresentation()
	let currentTabs = $state<Record<string, string>>({
		'nature-scorecard': 'nature-scorecard',
		'data-portal': 'data-portal',
		'country-profiles': 'country-profiles',
		scrollytelling: 'women-in-parliament'
	})
	let layout = $state('')
	let visibility = $state('')
	let stepNo = $state(0)



	function getCurrentUrl() {
		return outputUrls[currentOutput]?.[currentTabs[currentOutput]] || ''
	}

	async function animateAudienceSegments() {
		await audienceSegments.to({ x: 0, opacity: 1 }, { duration: 800 })
	}

	async function animateDataPoints() {
		await dataPoints.to({ scale: 1, opacity: 1 }, { duration: 600 })
	}

	async function animateImpactMetrics() {
		await impactMetrics.to({ y: 0, opacity: 1 }, { duration: 700 })
	}

	async function nextOutput() {
		currentIndex = (currentIndex + 1) % outputs.length
		currentOutput = outputs[currentIndex]
		await messagePersonalization.to({ x: 20, opacity: 0.8 }, { duration: 300 })
		await messagePersonalization.to({ x: 0, opacity: 1 }, { duration: 300 })
	}

	async function prevOutput() {
		currentIndex = (currentIndex - 1 + outputs.length) % outputs.length
		currentOutput = outputs[currentIndex]
		await messagePersonalization.to({ x: -20, opacity: 0.8 }, { duration: 300 })
		await messagePersonalization.to({ x: 0, opacity: 1 }, { duration: 300 })
	}

	function addAudience(event: KeyboardEvent) {
		if (event.key === 'Enter' && newAudienceInput.trim()) {
			customAudiences.push(newAudienceInput.trim())
			newAudienceInput = ''
			presentation.slides.nextFragment()
		} else if (
			event.key === 'Backspace' &&
			!newAudienceInput.trim() &&
			customAudiences.length > 0
		) {
			customAudiences.pop()
		}
	}

	function addDataOutput(event: KeyboardEvent) {
		if (event.key === 'Enter' && newDataOutputInput.trim()) {
			customDataOutputs.push(newDataOutputInput.trim())
			newDataOutputInput = ''
			presentation.slides.nextFragment()
		} else if (
			event.key === 'Backspace' &&
			!newDataOutputInput.trim() &&
			customDataOutputs.length > 0
		) {
			customDataOutputs.pop()
		}
	}
</script>

<Presentation
	options={{
		history: true,
		transition: 'slide',
		controls: true,
		progress: true,

		width: 1920,
		height: 1080
	}}
>
	<!-- Title & Introduction -->
	<Slide class="place-content-center place-items-center h-full">
		<Transition visible>
			<div class="space-y-8 text-center">
				<h1
					class="drop-shadow-lg mb-8 max-w-3xl font-bold text-white text-6xl text-center leading-17"
				>
					Audience Mapping <span class="text-[var(--cti-tertiary)]">
						& Tailoring Data Communication</span
					>
				</h1>
				<a
					class="font-bold text-white text-4xl hover:underline no-underline"
					href="https://www.linkedin.com/in/durand1/"
				>
					Durand D'souza
				</a>
			</div>
		</Transition>

		<Transition class="mt-16">
			<div class="text-center">
				<p class="mb-8 max-w-xl text-xl">
					Transforming complex data into compelling narratives for policymakers, investors, and the
					public
				</p>
			</div>
		</Transition>
	</Slide>

	<!-- Who We Are -->
	<Slide class="place-content-center place-items-center h-full">
		<Transition visible>
			<div class="space-y-12 max-w-5xl">
				<h2 class="mb-8 font-bold text-white text-5xl text-center">Who We Are</h2>

				<div>
					<p class="mb-6 text-white text-2xl text-center">
						Tracker Group is home to <span class="font-semibold text-[var(--cti-tertiary)]"
							>Carbon Tracker</span
						>
						and <span class="font-semibold text-[var(--cti-tertiary)]">Planet Tracker</span>,
						financial think tanks focusing on the climate nature nexus.
					</p>
				</div>

				<div class="gap-8 grid grid-cols-1 md:grid-cols-2">
					<Transition class="p-4 border-2">
						<h3 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">Mission</h3>
						<p class="text-white text-lg">
							transform global financial activities and reallocate capital to create a resilient,
							just, net zero and nature-positive future.
						</p>
					</Transition>

					<Transition class="p-4 border-2">
						<h3 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">Vision</h3>
						<p class="text-white text-lg">
							a global economy operating within climate & planetary boundaries.
						</p>
					</Transition>
				</div>
			</div>
		</Transition>
	</Slide>

	<!-- About Me -->
	<Slide class="justify-center place-content-center place-items-center h-full">
		<Transition visible>
			<div class="space-y-12 max-w-4xl text-center">
				<h2 class="mb-8 font-bold text-white text-5xl">About me</h2>

				<div class="space-y-8">
					<Transition>
						<h3 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-3xl">Durand D'souza</h3>
						<p class="mb-2 text-white text-xl">Senior Data Scientist</p>
						<p class="text-white/80 text-lg">
							Data & Modelling Team • <a
								class="font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
								href="https://carbontracker.org">Tracker Group</a
							>
						</p>
					</Transition>

					<div class="gap-8 grid grid-cols-1 md:grid-cols-2 mt-8 w-[60vw]">
						<Transition class="bg-white/5 p-4 border-l-2 h-[20ch]">
							<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">
								Recent projects
							</h4>
							<ul class="flex flex-col items-start gap-4 pl-4 text-lg xl:text-xl text-left">
								<Transition
									><li>
										Led the development of the Carbon Tracker <a
											class="font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
											href="https://data.carbontracker.org">data portal</a
										>
									</li></Transition
								>
								<Transition
									><li>Wrote reports on energy transition of various countries</li></Transition
								>
								<Transition
									><li>Led the development of global coal & gas economic models</li></Transition
								>
								<Transition><li>Financial modelling & scenario analysis</li></Transition>
								<Transition><li>Creating bespoke data visualisations</li></Transition>
								<Transition><li>Developing LLM tools for climate</li></Transition>
							</ul>
						</Transition>

						<Transition class="bg-white/5 p-4 border-l-2 h-[20ch]">
							<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">
								Independent work
							</h4>
							<ul
								class="flex flex-col items-start gap-4 pl-4 text-white text-lg xl:text-xl text-left"
							>
								<Transition
									><li>
										Freelance data journalism, specializing in long-form <a
											class="font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
											href="https://pudding.cool/2018/07/women-in-parliament/">data storytelling</a
										> and narrative-driven analysis.
									</li></Transition
								>
								<Transition>
									<li>
										Currently building <a
											class="font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
											href="https://github.com/dldx/climate-rag">Climate RAG</a
										>, a multi-lingual and multi-strategy approach to finding key data for the
										climate and energy sector.
									</li></Transition
								>
							</ul>
						</Transition>
					</div>
				</div>
			</div>
		</Transition>
	</Slide>
	<Slide class="place-content-center place-items-center h-full">
		<Transition visible>
			<h2 class="mb-12 text-5xl text-center">About you</h2>
			<div class="w-[95vw] h-[85vh]">
				<iframe
					src="https://link.excalidraw.com/l/6slCDyz3sXF/42irFElx8nP"
					title="Participants intro"
					width="100%"
					height="100%"
					style="border: none;"
				></iframe>
			</div>
		</Transition>
	</Slide>

	<!-- Understanding Your Audience Segments -->
	<Slide class="place-content-center place-items-center h-full" in={animateAudienceSegments}>
		<Transition visible>
			<h2 class="mb-8 text-5xl text-center">Know your target audiences</h2>
		</Transition>

		<div class="flex flex-wrap gap-2 max-w-5xl">
			<Transition class="bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">📰 Journalists</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Looking for novel angles</li>
						<li>Fast turnarounds</li>
						<li>Tie-ins with other stories</li>
						<li>Visual content or data visualisations preferred</li>
					</ul>
				</div>
			</Transition>

			<Transition class="flex-grow bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">🏛️ Policymakers</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Require actionable insights</li>
						<li>Focus on policy implications</li>
						<li>Need executive summaries</li>
						<li>Value cost-benefit analysis</li>
						<li>Bespoke targetted analysis could be more impactful</li>
					</ul>
				</div>
			</Transition>

			<Transition class="bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">
						💰 Financial institutions
					</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Focus on financial risks</li>
						<li>Want financial implications</li>
						<li>Need scenario analysis to inform decision-making</li>
						<li>Niche data but could be very impactful</li>
					</ul>
				</div>
			</Transition>

			<Transition class="flex-grow bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">👥 General Public</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Need clear-cut narratives</li>
						<li>Value personal relevance</li>
						<li>Respond to emotions</li>
						<li>Engage better with visual content</li>
					</ul>
				</div>
			</Transition>

			<Transition class="bg-white/5 p-8 border-l-2">
				<h4 class="font-semibold text-[var(--cti-tertiary)] text-2xl">🌍 NGOs</h4>
			</Transition>
			<!-- Render custom audiences -->
			{#each customAudiences as audience}
				<Transition class="bg-white/5 p-8 border-l-2" visible={!hideCustomAudiences}>
					<h4 class="font-semibold text-[var(--cti-tertiary)] text-2xl">👥 {audience}</h4>
				</Transition>
			{/each}
			<!-- Add new audience input -->
			<Transition
				class="bg-white/5 p-5 border-l-2 max-w-[13ch]"
				undo={() => (hideCustomAudiences = true)}
				do={() => (hideCustomAudiences = false)}
			>
				<input
					type="text"
					bind:value={newAudienceInput}
					onkeydown={addAudience}
					placeholder="Any others?"
					class="bg-white/10 p-2 border border-white/20 focus:border-[var(--cti-tertiary)] focus:outline-none w-full text-white text-3xl font-(family-name:--r-heading-font) placeholder-white/60"
				/>
			</Transition>
		</div>
	</Slide>

	<!-- Know your data outputs -->
	<Slide class="place-content-center place-items-center h-full">
		<Transition visible>
			<h2 class="mb-8 text-5xl text-center">Data outputs & mediums</h2>
		</Transition>

		<div class="flex flex-wrap gap-2 max-w-5xl">
			<Transition class="flex-grow bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">📊 Reports</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Static</li>
						<li>Easy to make publication-ready</li>
						<li>Shareable as PDFs</li>
						<li>Easy to print & distribute physically</li>
					</ul>
				</div>
			</Transition>
			<Transition class="flex-grow bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">🎨 Infographics</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Visual storytelling</li>
						<li>Simplifies complex data</li>
						<li>Social media friendly</li>
						<li>High engagement potential</li>
						<li>
							<ul class="flex gap-2 pt-2 text-sm list-none">
								<li>
									<a
										href="https://www.canva.com/create/infographics/"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Canva</a
									>
								</li>
								<li>
									<a
										href="https://www.figma.com"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Figma</a
									>
								</li>
								<li>
									<a
										href="https://inkscape.org"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Inkscape</a
									>
								</li>
							</ul>
						</li>
					</ul>
				</div>
			</Transition>

			<Transition class="flex-grow bg-white/5 p-8 border-l-2 max-w-[20ch]">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">
						🎛️ Interactive Dashboards
					</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Real-time data exploration</li>
						<li>User-driven analysis = active engagement</li>
						<li>May not be easy to make publication-ready</li>
						<li>Different takeaways for each user (?)</li>
						<li>
							<ul class="flex gap-2 pt-2 text-sm list-none">
								<li>
									<a
										href="https://dash.plotly.com"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Plotly Dash</a
									>
								</li>
								<li>
									<a
										href="https://www.tableau.com/"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Tableau</a
									>
								</li>
								<li>
									<a
										href="https://www.microsoft.com/en-us/power-platform/products/power-bi"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Power BI</a
									>
								</li>
							</ul>
						</li>
					</ul>
				</div>
			</Transition>

			<Transition class="flex-grow bg-white/5 p-8 border-l-2">
				<div class="space-y-4">
					<h4 class="mb-4 font-semibold text-[var(--cti-tertiary)] text-2xl">
						💬 Targetted explorables
					</h4>
					<ul class="space-y-2 text-white text-lg xl:text-xl text-left">
						<li>Stakeholder-focused</li>
						<li>Actionable recommendations</li>
						<li>Key charts & tables only</li>
						<li>Policy implications</li>
						<li>
							<ul class="flex gap-2 pt-2 text-sm list-none">
								<li>
									<a
										href="https://svelte.dev"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Svelte</a
									>
								</li>
								<li>
									<a
										href="https://dash.plotly.com"
										class="bg-white/5 p-2 font-semibold text-[var(--cti-tertiary)] hover:underline no-underline"
										target="_blank">Plotly Dash</a
									>
								</li>
							</ul>
						</li>
					</ul>
				</div>
			</Transition>

			<!-- Render custom data outputs -->
			{#each customDataOutputs as output}
				<Transition class="bg-white/5 p-8 border-l-2" visible={!hideCustomDataOutputs}>
					<h4 class="font-semibold text-[var(--cti-tertiary)] text-2xl">🔧 {output}</h4>
				</Transition>
			{/each}

			<!-- Add new data output input -->
			<Transition
				class="bg-white/5 p-5 border-l-2 max-w-[13ch]"
				undo={() => (hideCustomDataOutputs = true)}
				do={() => (hideCustomDataOutputs = false)}
			>
				<input
					type="text"
					bind:value={newDataOutputInput}
					onkeydown={addDataOutput}
					placeholder="What's missing?"
					class="bg-white/10 p-2 border border-white/20 focus:border-[var(--cti-tertiary)] focus:outline-none w-full text-white text-3xl font-(family-name:--r-heading-font) placeholder-white/60"
				/>
			</Transition>
		</div>
	</Slide>

	<!-- Case studies -->
	<Slide
		class="place-content-center place-items-center cti-bg-secondary h-full"
		in={animateDataPoints}
	>
		<Transition visible>
			<h2 class="mb-12 font-bold text-5xl text-center">Case studies</h2>
		</Transition>
	</Slide>

	<!-- Multiple Data Outputs -->
	<Slide class="place-content-center place-items-center cti-bg-tertiary h-full">
		<div class="flex flex-row items-center gap-2 w-[90vw]">
			<div
				class="bg-white shadow-xl p-8 border-teal-500 border-l-4 w-[20vw] h-fit"
				style="transform: translateX({messagePersonalization.x}px); opacity: {messagePersonalization.opacity}"
			>
				<div class="flex justify-between items-center mb-6">
					<h3 class="font-semibold text-gray-800 text-xl text-left capitalize">
						{currentOutput.replace('-', ' ')}
					</h3>
					<div class="flex gap-2">
						<Button variant="secondary" size="icon" class="size-8" onclick={prevOutput}>
							<ChevronLeftIcon />
						</Button>
						<Button variant="secondary" size="icon" class="size-8" onclick={nextOutput}>
							<ChevronRightIcon />
						</Button>
					</div>
				</div>

				{#if currentOutput === 'nature-scorecard'}
					<p class="gap-x-4 grid grid-cols-[1fr_auto] text-gray-600 text-sm text-left">
						<strong>Format</strong> Tableau dashboard
						<strong>Audience</strong> Institutional investors, sustainability analysts
						<strong>Dev time</strong> 🕐
					</p>
					<div class="bg-blue-50 mt-4 py-2 border border-blue-200 text-center">
						<p class="font-medium text-blue-800 text-sm">
							<img
								src="https://planet-tracker.org/wp-content/uploads/2023/11/pt-fav.png"
								alt="Planet Tracker Logo"
								class="inline-block w-4 h-4"
							/>
							<a
								href="https://planet-tracker.org/nature-scorecard/"
								class="underline"
								target="_blank">planet-tracker.org/nature-scorecard</a
							>
						</p>
					</div>
				{:else if currentOutput === 'country-profiles'}
					<p class="gap-x-4 grid grid-cols-[1fr_auto] text-gray-600 text-sm text-left">
						<strong>Format:</strong> Country-specific policy dashboards
						<strong>Audience:</strong> Policymakers, diplomats, government analysts
						<strong>Dev time</strong> 🕐🕐
					</p>
					<div class="bg-blue-50 mt-4 py-2 border border-blue-200 text-center">
						<p class="font-medium text-green-800 text-sm">
							<img
								src="https://carbontracker.org/wp-content/uploads/2017/09/cropped-Carbon_Tracker_PRI_Favicon_v1.0-180x180.png"
								alt="Carbon Tracker Logo"
								class="inline-block w-4 h-4"
							/>
							<a href="https://countryprofiles.carbontracker.org" class="underline" target="_blank"
								>countryprofiles.carbontracker.org</a
							>
						</p>
					</div>
				{:else if currentOutput === 'scrollytelling'}
					<p class="gap-x-4 grid grid-cols-[1fr_auto] text-gray-600 text-sm text-left">
						<strong>Format</strong> Narrative-driven data storytelling
						<strong>Audience</strong> General public, NGOs, policymakers (?)
						<strong>Dev time</strong> 🕐🕐 - 🕐🕐🕐🕐🕐
					</p>
					<div class="bg-blue-50 mt-4 py-2 border border-blue-200 text-center">
						<p class="font-medium text-green-800 text-sm">
							<img
								src="https://pudding.cool/favicon.ico"
								alt="Pudding Logo"
								class="inline-block w-4 h-4"
							/>
							<a
								href="https://pudding.cool/2018/07/women-in-parliament"
								class="underline"
								target="_blank">pudding.cool/2018/07/women-in-parliament</a
							>
						</p>
					</div>
				{:else if currentOutput === 'data-portal'}
					<p class="gap-x-4 grid grid-cols-[1fr_auto] text-gray-600 text-sm text-left">
						<strong>Format</strong> Interactive data exploration platform
						<strong>Audience</strong> Data analysts, researchers, journalists, NGOs
						<strong>Dev time</strong> 🕐🕐🕐🕐
					</p>
					<div class="bg-blue-50 mt-4 p-0 py-2 border border-blue-200 text-center">
						<p class="font-medium text-blue-800 text-sm">
							<img
								src="https://carbontracker.org/wp-content/uploads/2017/09/cropped-Carbon_Tracker_PRI_Favicon_v1.0-180x180.png"
								alt="Carbon Tracker Logo"
								class="inline-block w-4 h-4"
							/>
							<a href="https://data.carbontracker.org" class="underline" target="_blank"
								>data.carbontracker.org</a
							>
						</p>
					</div>
				{/if}
			</div>
			<div class="flex flex-col flex-grow bg-muted shadow-xl p-0 h-[90vh]">
				<div>
					<Tabs.Root bind:value={currentTabs[currentOutput]} class="w-full">
						<Tabs.List
							class="grid w-full rounded-none p-0 grid-cols-{Object.keys(
								outputUrls[currentOutput] || {}
							).length}"
						>
							{#each Object.keys(outputUrls[currentOutput] || {}) as tabKey}
								<Tabs.Trigger
									value={tabKey}
									class="bg-[var(--r-background-color)] dark:data-[state=active]:bg-[var(--cti-primary)] rounded-none capitalize"
								>
									{tabKey.replaceAll('-', ' ')}
								</Tabs.Trigger>
							{/each}
						</Tabs.List>
					</Tabs.Root>
				</div>
				<iframe
					class="flex-grow border-none w-full"
					src={getCurrentUrl()}
					title="{currentOutput} preview"
					height="100%"
					width="100%"
				></iframe>
			</div>
		</div>

		<!-- Add Action components for navigation -->
		<!-- <Action do={cycleOutputs} />
		<Action do={cycleOutputs} />
		<Action do={cycleOutputs} />
		<Action do={cycleOutputs} /> -->
	</Slide>
	<!-- Helping participants understand the audience mapping process -->
	<Slide class="place-content-center place-items-center h-full">
		<Transition visible>
			<div class="w-[98vw] h-[97vh]">
				<iframe
					src="https://link.excalidraw.com/l/6slCDyz3sXF/8OLVxQm5nGQ"
					title="Audience Mapping"
					width="100%"
					height="100%"
					style="border: none;"
				></iframe>
			</div>
		</Transition>
	</Slide>

	<!-- Slide 5: Measuring Impact & Next Steps -->
	<Slide
		class="place-content-center place-items-center cti-bg-accent h-full"
	>
		<div
			class="space-y-8 max-w-6xl"
		>
			<div class="bg-gradient-to-r from-emerald-600 to-teal-600 p-8 rounded-xl text-white">
				<h3 class="mb-6 font-bold text-3xl text-center">🚀 Your Action Plan</h3>
				<div class="gap-6 grid grid-cols-3 text-center align-top">
					<Transition class="col-span-3 text-center" order={4}>
						<div class="bg-white shadow-lg mx-auto p-2 border-2 rounded-sm w-full">
							<span
								class="inline-flex justify-center items-center bg-[var(--cti-tertiary)] mr-3 rounded w-8 h-8 font-bold text-white text-2xl"
								>0</span
							>
							<span class="font-semibold text-gray-700 text-xl"
								>Plan ahead. Prioritise reusable components.</span
							>
						</div>
					</Transition>
					<Transition order={1}>
						<div class="space-y-2 min-h-[5ch]">
							<div
								class="inline-flex justify-center items-center bg-[var(--cti-tertiary)] rounded w-8 h-8 font-bold text-white text-2xl"
							>
								1
							</div>
							<h4 class="font-semibold text-lg">Map Your Audiences</h4>
							<p class="text-emerald-100 text-sm">Identify & segment your key stakeholders</p>
						</div></Transition
					><Transition order={2}>
						<div class="space-y-2">
							<div
								class="inline-flex justify-center items-center bg-[var(--cti-tertiary)] rounded w-8 h-8 font-bold text-white text-2xl"
							>
								2
							</div>
							<h4 class="font-semibold text-lg">Tailor Your Content</h4>
							<p class="text-emerald-100 text-sm">Choose a medium. Adapt complexity & messaging.</p>
						</div></Transition
					><Transition order={3}>
						<div class="space-y-2">
							<div
								class="inline-flex justify-center items-center bg-[var(--cti-tertiary)] rounded w-8 h-8 font-bold text-white text-2xl"
							>
								3
							</div>
							<h4 class="font-semibold text-lg">Measure & Iterate</h4>
							<p class="text-emerald-100 text-sm">Track impact & build on what works.</p>
						</div></Transition
					>
				</div>
			</div>
		</div>
	</Slide>
	<Slide
		class="place-content-center place-items-center h-full"
		in={() => (layout = '')}
		out={() => {
			layout = ''
		}}
	>
		<Transition visible>
			<h1 class="static !text-yellow-300 text-5xl">Automated impact tracking</h1>
		</Transition>
		<Transition
			enter="enter"
			class="hidden"
			order={1}
			do={() => {
				layout = 'h-[70vh]'
				visibility = ''
				stepNo = 1
			}}
			undo={() => {
				layout = ''
				visibility = ''
			}}
		>
			<div class="{layout} m-16 flex flex-col place-content-center place-items-center">
				<div
					class="{visibility} flex max-w-[50ch] flex-col place-content-center place-items-center text-sm xl:text-xl"
				>
					<Transition enter="fade-enter" class="mt-16">
						By using a combination of LLMs and RAG (Retrieval Augmented Generation), we can begin to track the impact of our work.
						We do this by assessing whether recommendations made in our reports are being implemented by our target audiences.
					</Transition>
				</div>
				<Transition
					enter="fade-enter"
					class="w-[80vw] xl:w-[85vw] h-[80vh] xl:h-[85vh]"
					do={() => {
						visibility = 'hidden'
						stepNo = 0
						layout = 'h-[70vh]'
					}}
					undo={() => {
						visibility = ''
						stepNo = 0
						layout = 'h-[70vh]'
					}}
				>
					<RagPipeline {stepNo} />
				</Transition>
			</div>
		</Transition>
	</Slide>

</Presentation>
