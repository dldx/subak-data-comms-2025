<script lang="ts">
	import { Presentation, Slide, Transition, Action, getPresentation } from '@animotion/core'
	import { tween } from '@animotion/motion'
	import * as Tabs from "$lib/components/ui/tabs/index.js"
	import { Button } from "$lib/components/ui/button/index.js"
	import ChevronRightIcon from "@lucide/svelte/icons/chevron-right"

	let audienceSegments = tween({ x: -400, opacity: 0 })
	let dataPoints = tween({ scale: 0, opacity: 0 })
	let impactMetrics = tween({ y: 100, opacity: 0 })
	let messagePersonalization = tween({ x: 0, opacity: 1 })

	let currentOutput = $state('nature-scorecard')
	let outputs = ['nature-scorecard', 'country-profiles', 'scrollytelling', 'data-portal']
	let currentIndex = $state(0)

	const outputUrls: Record<string, Record<string, string>> = {
		'nature-scorecard': {
			'nature-scorecard': 'https://planet-tracker.org/nature-scorecard/',
		},
		'data-portal': {
			'data-portal': 'https://data.carbontracker.org',
		},
		'country-profiles': {
			'country-profiles': 'https://countryprofiles.carbontracker.org',
		},
		'scrollytelling': {
			'women-in-parliament': 'https://pudding.cool/2018/07/women-in-parliament',
			'japan-carbon-pricing': 'https://carbon-tracker-initiative.github.io/japan-interactive-2022/'
		}
	}
	let customAudiences = $state<string[]>([])
	let newAudienceInput = $state('')
	let hideCustomAudiences = $state(false)
	let presentation = getPresentation()
	let currentTabs = $state<Record<string, string>>({
		'nature-scorecard': 'nature-scorecard',
		'data-portal': 'data-portal',
		'country-profiles': 'country-profiles',
		'scrollytelling': 'women-in-parliament'
	})

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

	async function cycleOutputs() {
		currentIndex = (currentIndex + 1) % outputs.length
		currentOutput = outputs[currentIndex]
		await messagePersonalization.to({ x: 20, opacity: 0.8 }, { duration: 300 })
		await messagePersonalization.to({ x: 0, opacity: 1 }, { duration: 300 })
	}

	function addAudience(event: KeyboardEvent) {
		if (event.key === 'Enter' && newAudienceInput.trim()) {
			customAudiences.push(newAudienceInput.trim())
			newAudienceInput = ''
			presentation.slides.nextFragment()
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
			<h2 class="mb-12 text-5xl text-center">Know your target audiences</h2>
		</Transition>

		<div class="flex flex-wrap gap-8 max-w-5xl">
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
						<li>Focused on financial risks</li>
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
			<Transition class="bg-white/5 p-5 border-l-2 max-w-[13ch]" undo={() => hideCustomAudiences = true} do={() => hideCustomAudiences = false}>
					<input
						type="text"
						bind:value={newAudienceInput}
						onkeydown={addAudience}
						placeholder="Insert audience..."
						class="bg-white/10 p-2 border border-white/20 focus:border-[var(--cti-tertiary)] focus:outline-none w-full font-[var(--r-heading-font)] text-white text-3xl placeholder-white/60"
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
			<h2 class="mb-12 font-bold text-5xl text-center">
				Case studies
			</h2>
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
						<Button variant="secondary" size="icon" class="size-8" onclick={cycleOutputs}>
							<ChevronRightIcon />
						</Button>
					</div>

					{#if currentOutput === 'nature-scorecard'}
							<p class="gap-x-4 grid grid-cols-[1fr_auto] text-gray-600 text-sm text-left">
								<strong>Format</strong> Tableau dashboard
								<strong>Audience</strong> Institutional investors, sustainability analysts
								<strong>Dev time</strong> 🕐
							</p>
							<div class="bg-blue-50 mt-4 py-2 border border-blue-200 text-center">
								<p class="font-medium text-blue-800 text-sm">
									<img src="https://planet-tracker.org/wp-content/uploads/2023/11/pt-fav.png" alt="Planet Tracker Logo" class="inline-block w-4 h-4" />
									<a href="https://planet-tracker.org/nature-scorecard/" class="underline" target="_blank"
										>planet-tracker.org/nature-scorecard</a
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
									<img src="https://carbontracker.org/wp-content/uploads/2017/09/cropped-Carbon_Tracker_PRI_Favicon_v1.0-180x180.png" alt="Carbon Tracker Logo" class="inline-block w-4 h-4" />
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
									<img src="https://pudding.cool/favicon.ico" alt="Pudding Logo" class="inline-block w-4 h-4" />
									<a href="https://pudding.cool/2018/07/women-in-parliament" class="underline" target="_blank"
										>pudding.cool/2018/07/women-in-parliament</a
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
									<img src="https://carbontracker.org/wp-content/uploads/2017/09/cropped-Carbon_Tracker_PRI_Favicon_v1.0-180x180.png" alt="Carbon Tracker Logo" class="inline-block w-4 h-4" />
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
							<Tabs.List class="p-0 grid w-full rounded-none grid-cols-{Object.keys(outputUrls[currentOutput] || {}).length}">
								{#each Object.keys(outputUrls[currentOutput] || {}) as tabKey}
									<Tabs.Trigger value={tabKey} class="bg-[var(--r-background-color)] dark:data-[state=active]:bg-[var(--cti-primary)] rounded-none capitalize">
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
					src="https://link.excalidraw.com/l/6slCDyz3sXF/42irFElx8nP"
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
		in={animateImpactMetrics}
	>
		<Transition visible>
			<h2 class="mb-12 font-bold text-emerald-800 text-5xl text-center">
				📈 Measuring Communication Impact
			</h2>
		</Transition>

		<div
			class="space-y-8 max-w-6xl"
			style="transform: translateY({impactMetrics.y}px); opacity: {impactMetrics.opacity}"
		>
			<div class="gap-8 grid grid-cols-2">
				<Transition class="bg-white shadow-lg p-8 border-emerald-500 border-l-4 rounded-xl">
					<h3 class="mb-6 font-semibold text-emerald-800 text-2xl">📊 Quantitative Metrics</h3>
					<div class="space-y-3">
						<div class="flex justify-between items-center bg-emerald-50 p-3 rounded-lg">
							<span class="text-emerald-700">Engagement Rate</span>
							<span class="font-bold text-emerald-800">↗️ Track clicks, shares, time on page</span>
						</div>
						<div class="flex justify-between items-center bg-emerald-50 p-3 rounded-lg">
							<span class="text-emerald-700">Reach & Distribution</span>
							<span class="font-bold text-emerald-800">📢 Monitor audience size growth</span>
						</div>
						<div class="flex justify-between items-center bg-emerald-50 p-3 rounded-lg">
							<span class="text-emerald-700">Content Performance</span>
							<span class="font-bold text-emerald-800">🎯 A/B test different approaches</span>
						</div>
					</div>
				</Transition>

				<Transition class="bg-white shadow-lg p-8 border-blue-500 border-l-4 rounded-xl">
					<h3 class="mb-6 font-semibold text-blue-800 text-2xl">💬 Qualitative Feedback</h3>
					<div class="space-y-3">
						<div class="flex justify-between items-center bg-blue-50 p-3 rounded-lg">
							<span class="text-blue-700">Audience Surveys</span>
							<span class="font-bold text-blue-800">📝 Direct feedback collection</span>
						</div>
						<div class="flex justify-between items-center bg-blue-50 p-3 rounded-lg">
							<span class="text-blue-700">Stakeholder Interviews</span>
							<span class="font-bold text-blue-800">🗣️ Deep-dive conversations</span>
						</div>
						<div class="flex justify-between items-center bg-blue-50 p-3 rounded-lg">
							<span class="text-blue-700">Comment Analysis</span>
							<span class="font-bold text-blue-800">💭 Social listening & sentiment</span>
						</div>
					</div>
				</Transition>
			</div>

			<Transition class="bg-gradient-to-r from-emerald-600 to-teal-600 p-8 rounded-xl text-white">
				<h3 class="mb-6 font-bold text-3xl text-center">🚀 Your Action Plan</h3>
				<div class="gap-6 grid grid-cols-3 text-center">
					<div class="space-y-2">
						<div class="text-4xl">1️⃣</div>
						<h4 class="font-semibold text-lg">Map Your Audiences</h4>
						<p class="text-emerald-100">Identify & segment your key stakeholders</p>
					</div>
					<div class="space-y-2">
						<div class="text-4xl">2️⃣</div>
						<h4 class="font-semibold text-lg">Tailor Your Content</h4>
						<p class="text-emerald-100">Adapt complexity & messaging per audience</p>
					</div>
					<div class="space-y-2">
						<div class="text-4xl">3️⃣</div>
						<h4 class="font-semibold text-lg">Measure & Iterate</h4>
						<p class="text-emerald-100">Track impact & continuously improve</p>
					</div>
				</div>
			</Transition>

			<Transition class="text-center">
				<div class="bg-white shadow-lg p-6 border-2 border-emerald-200 rounded-xl">
					<p class="mb-4 text-gray-700 text-xl">
						<strong>Remember:</strong> Effective data communication bridges the gap between insight and
						action
					</p>
					<p class="text-emerald-600 text-lg">
						Questions? Let's discuss how to implement these strategies for your organization.
					</p>
				</div>
			</Transition>
		</div>
	</Slide>
</Presentation>
