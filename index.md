---
layout: default
title: Home
---
<div class="home">

  <h1 class="page-heading">Meetings</h1>

  {%- assign allPosts = site.posts -%}
  {%- assign currentYear = "now" | date: "%Y" -%}
  
  <!-- Create array of unique years from posts -->
  {%- assign years = "" -%}
  {%- for post in allPosts -%}
    {%- assign postYear = post.date | date: "%Y" -%}
    {%- unless years contains postYear -%}
      {%- assign years = years | append: postYear | append: "," -%}
    {%- endunless -%}
  {%- endfor -%}
  {%- assign yearArray = years | split: "," | sort | reverse -%}

  <!-- Two-column layout container -->
  <div class="content-container">
    
    <!-- Left sidebar with year filter menu -->
    <aside class="sidebar">
      <div class="year-menu">
        <h3>Filter by Year</h3>
        <div class="year-buttons">
          <button class="year-btn active" data-year="all" onclick="filterByYear('all')">All Years</button>
          {%- for year in yearArray -%}
            {%- if year != "" -%}
              <button class="year-btn" data-year="{{ year }}" onclick="filterByYear('{{ year }}')">{{ year }}</button>
            {%- endif -%}
          {%- endfor -%}
        </div>
      </div>
    </aside>

    <!-- Main content area with posts -->
    <main class="main-content">
      <!-- Posts count display -->
      <p id="posts-count">There are {{ allPosts.size }} meetings.</p>

      <!-- Posts list -->
      <ul class="post-list">
        {% for post in allPosts %}
          {%- assign postYear = post.date | date: "%Y" -%}
          <li class="post-item" data-year="{{ postYear }}">
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

            <h2>
              <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
            </h2>
            
            {%- if post.summary -%}
              <p class="post-excerpt">{{ post.summary }}</p>
            {%- endif -%}
          </li>
        {% endfor %}
      </ul>

      <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>
    </main>
    
  </div>

</div>

<style>
  /* Two-column layout */
  .content-container {
    display: flex;
    gap: 2em;
    align-items: flex-start;
  }
  
  /* Left sidebar */
  .sidebar {
    flex: 0 0 150px; /* Fixed width sidebar */
    position: sticky;
    top: 20px; /* Stick to top when scrolling */
  }
  
  /* Main content area */
  .main-content {
    flex: 1; /* Take remaining space */
    min-width: 0; /* Allow content to shrink */
  }
  
  /* Year menu styling */
  .year-menu {
    background-color: #f8f9fa;
    border: 2px solid #2c5aa0;
    border-radius: 8px;
    padding: 1em;
  }
  
  .year-menu h3 {
    margin-top: 0;
    margin-bottom: 1em;
    color: #2c5aa0;
    font-size: 1.1em;
  }
  
  .year-buttons {
    display: flex;
    flex-direction: column; /* Stack buttons vertically */
    gap: 0.5em;
  }
  
  .year-btn {
    background-color: #ffffff;
    border: 2px solid #2c5aa0;
    color: #2c5aa0;
    padding: 0.8em 1em;
    cursor: pointer;
    border-radius: 4px;
    font-weight: bold;
    transition: all 0.3s ease;
    text-align: center;
    width: 100%;
  }
  
  .year-btn:hover {
    background-color: #2c5aa0;
    color: white;
    transform: translateY(-1px);
    box-shadow: 0 2px 4px rgba(44, 90, 160, 0.3);
  }
  
  .year-btn.active {
    background-color: #2c5aa0;
    color: white;
    box-shadow: 0 2px 4px rgba(44, 90, 160, 0.3);
  }
  
  /* Post items styling */
  .post-item {
    margin-bottom: .75em;
    padding-bottom: 1em;
    border-bottom: 1px solid #eee;
  }
  
  .post-item.hidden {
    display: none;
  }
  
  .post-excerpt {
    color: #666;
    font-style: italic;
    margin-top: 0.2em;
  }
  
  /* Posts count styling */
  #posts-count {
    font-weight: bold;
    color: #2c5aa0;
    margin-bottom: 1.5em;
    padding-bottom: 0.5em;
    border-bottom: 2px solid #2c5aa0;
  }
  
  /* Responsive design */
  @media (max-width: 768px) {
    .content-container {
      flex-direction: column;
      gap: 1em;
    }
    
    .sidebar {
      flex: none;
      position: static;
      width: 100%;
    }
    
    .year-buttons {
      flex-direction: row;
      flex-wrap: wrap;
    }
    
    .year-btn {
      flex: 1;
      min-width: 80px;
    }
  }
</style>

<script>
  function filterByYear(selectedYear) {
    // Update button states
    const buttons = document.querySelectorAll('.year-btn');
    buttons.forEach(btn => {
      btn.classList.remove('active');
      if (btn.dataset.year === selectedYear) {
        btn.classList.add('active');
      }
    });
    
    // Filter posts
    const posts = document.querySelectorAll('.post-item');
    let visibleCount = 0;
    
    posts.forEach(post => {
      const postYear = post.dataset.year;
      if (selectedYear === 'all' || postYear === selectedYear) {
        post.classList.remove('hidden');
        visibleCount++;
      } else {
        post.classList.add('hidden');
      }
    });
    
    // Update posts count
    const countEl = document.getElementById('posts-count');
    if (selectedYear === 'all') {
      countEl.textContent = `There are ${visibleCount} meetings.`;
    } else {
      countEl.textContent = `There are ${visibleCount} meetings in ${selectedYear}.`;
    }
  }
  
  // Set default to current year on page load
  document.addEventListener('DOMContentLoaded', function() {
    const currentYear = new Date().getFullYear().toString();
    const currentYearBtn = document.querySelector(`[data-year="${currentYear}"]`);
    
    if (currentYearBtn) {
      // Default to current year if it exists
      filterByYear(currentYear);
    } else {
      // If current year doesn't exist, default to "All Years"
      filterByYear('all');
    }
  });
</script>