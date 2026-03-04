<!--
@component
This is your page!
-->
<script>
  // Import all the news furniture components
  import ArticleHeader from '$lib/components/ArticleHeader.svelte';
  import ArticleBody from '$lib/components/ArticleBody.svelte';
  import Image from '$lib/components/Image.svelte';
  import RelatedLinks from '$lib/components/RelatedLinks.svelte';
  import RestaurantTable from '$lib/components/RestaurantTable.svelte';
  import BigNumber from '$lib/components/BigNumber.svelte';
  import Dashboard from '$lib/components/Dashboard.svelte';

  // Article metadata
  let headline = 'Restaurants. They\'re back and they\'re filterable.';
  let byline = 'NYCity News Service';
  let pubDate = '2026-01-31';

  // Related stories
  const relatedStories = [
    { headline: 'How America\'s top news organizations escape rigid publishing systems to design beautiful data-driven stories on deadline.', href: 'https://palewi.re/docs/coding-the-news/' },
    { headline: 'How to install, configure and use Visual Studio Code, GitHub and Copilot', href: 'https://palewi.re/docs/coding-the-news/scripts/week-1/' },
    { headline: "How to publish a website with Node.JS and GitHub Actions", href:"https://palewi.re/docs/coding-the-news/scripts/week-2/"},
  ];

  // Importing restaurant data
    let { data } = $props();

  // Set the boro pulldown filter
    let selectedBorough = $state("");
    let selectedCuisine = $state("");
    let selectedGrade = $state("");

  // Set the cuisine filter
    let cuisines = $derived(
  [...new Set(data.restaurants.map(r => r.cuisine_description))].sort()
  );

  // Set the grade filter
  let grades = $derived(
  [...new Set(data.restaurants.map(r => r.grade))].sort()
  );

  //Making searchable and filterable
    let searchQuery = $state("");
    let restaurants = $derived(
    data.restaurants.filter(r => {
      if (selectedBorough !== '' && r.boro !== selectedBorough) return false;
      if (selectedCuisine !== '' && r.cuisine_description !== selectedCuisine) return false;
      if (selectedGrade !== '' && r.grade !== selectedGrade) return false;
      if (searchQuery !== '' && !r.dba.toLowerCase().includes(searchQuery.toLowerCase())) return false;
      return true;
    })
    );

    let displayed = $derived(restaurants.slice(0, 100));
    let displayedGradeA = $derived(restaurants.filter(r => r.grade === 'A'));
    let displayedGradeB = $derived(restaurants.filter(r => r.grade === 'B'));
    let displayedGradeC = $derived(restaurants.filter(r => r.grade === 'C'));
</script>

<!-- This sets the page title in the browser tab -->
<svelte:head>
  <title>{headline} | NYCity News Service</title>
  <meta name="description" content="At the Craig Newmark Graduate School of Journalism at the City University of New York, change is in our DNA. That comes of being born in 2006, as the digital revolution was transforming our profession in ways none of us could have imagined." />
</svelte:head>

<!-- Your page content goes here -->

  <!-- Article Header: Headline, byline, and publication date -->
  <div class="header-wrapper">
    <ArticleHeader
      {headline}
      {byline}
      {pubDate}
    />
  </div>

  <!-- Lead Image: Animated gif of students at the journalism school -->
  <Image
    src="/example-photo.gif"
    alt="The Craig Newmark Graduate School of Journalism is at 219 West 40th Street in Midtown Manhattan."
    caption="The Craig Newmark Graduate School of Journalism is at 219 West 40th Street in Midtown Manhattan."
    credit="Craig Newmark Graduate School of Journalism"
  />

  <!-- Article Body: The main story text with proper typography -->
  <ArticleBody>

  <Dashboard>
  <BigNumber
    number={displayedGradeA.length}
    label="Graded 'A'"
    footnote="As of March 2, 2026"
  />

  <BigNumber
    number={displayedGradeB.length}
    label="Graded 'B'"
    footnote="As of March 2, 2026"
  />

  <BigNumber
    number={displayedGradeC.length}
    label="Graded 'C'"
    footnote="As of March 2, 2026"
  />
</Dashboard>

  <div class="filters">
  <label for="borough">Borough</label>
  <select id="borough" bind:value={selectedBorough}>
    <option value="">All boroughs</option>
    <option value="Manhattan">Manhattan</option>
    <option value="Brooklyn">Brooklyn</option>
    <option value="Queens">Queens</option>
    <option value="Bronx">Bronx</option>
    <option value="Staten Island">Staten Island</option>
  </select>

  <label for="cuisine">Cuisine</label>
  <select id="cuisine" bind:value={selectedCuisine}>
    <option value="">All cuisines</option>
    {#each cuisines as cuisine}
      <option value={cuisine}>{cuisine}</option>
    {/each}
  </select>

  <label for="grade">Grade</label>
  <select id="grade" bind:value={selectedGrade}>
    <option value="">All grades</option>
    {#each grades as grade}
      <option value={grade}>{grade}</option>
    {/each}
  </select>

  <div>
    <label for="search">Search by name</label>
    <input id="search" type="text" bind:value={searchQuery} placeholder="e.g. Pizza" />
  </div>
</div>

<p class="count">Showing {displayed.length} of {restaurants.length} restaurants</p>

<RestaurantTable data={displayed} />

<hr>

    <p>
      At the Craig Newmark Graduate School of Journalism at the City University of New York, change is in our DNA. That comes of being born in 2006, as the digital revolution was transforming our profession in ways none of us could have imagined.
    </p>

    <p>
      We fashioned a school to teach the latest storytelling, entrepreneurial, and technological skills alongside reporting, writing, and ethics. Beyond that, we’ve crafted a culture that spurns complacency, that isn’t afraid to pivot before the ground under us shifts.
    </p>

    <p>
      Our mission is to serve the public interest – by training new journalists from varied economic, racial, and cultural backgrounds who will bring much-needed diversity to newsrooms, by helping mid-career journalists retool their skills, and by partnering with other media organizations to find new paths to excellence.
    </p>

    <p>
      Our low tuition rates, along with the added backing of private donors, allow candidates for our master’s degrees in journalism and engagement journalism to receive a world-class education at an affordable price. We also offer a unique bilingual master’s in journalism for students fluent in English and Spanish.
    </p>

    <p>
      Our three media centers provide research, training, thought leadership, industry meet-ups, and financial support for quality journalistic work.
    </p>

    <p>
      We also offer a robust professional education program through regular evening and weekend workshops. And we support in-depth reporting projects of professional journalists through fellowship grants.
    </p>

    <p>
      Classes are led by accomplished full-time faculty and adjuncts, who tap their networks to help students and graduates find internships, freelance opportunities and — the ultimate prize — jobs.
    </p>

    <p>
      At a time when our profession is reeling from financial pressures and lack of trust, the Newmark Graduate School of Journalism is committed to producing the next generation of skilled, ethically minded, and diverse journalists.
    </p>

    <p>
      We invite you to be part of our world.
    </p>
  </ArticleBody>

  <!-- Related Stories: Links to other articles -->
  <RelatedLinks
    title="Related Stories"
    links={relatedStories}
  />

<style>
  .filters {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    align-items: flex-end;
    padding: 1rem;
    background: #f8f8f8;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    margin-bottom: 0.75rem;
  }

  label {
    display: block;
    font-size: 0.75rem;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.25rem;
  }

  select {
    display: block;
    padding: 0.375rem 0.5rem;
    font-size: 0.875rem;
    border: 1px solid #ccc;
    border-radius: 3px;
  }

  input[type="text"] {
  display: block;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  border: 1px solid #ccc;
  border-radius: 3px;
}

  .count {
    font-size: 0.8rem;
    color: #888;
    margin: 0 0 0.5rem;
  }

  /* Styles here only apply to this page */
  .container {
    padding: var(--spacing-lg) var(--spacing-md);
  }

  .header-wrapper {
    padding-top: 2rem;
  }
</style>
