---
layout: home
---
<link rel="shortcut icon" href="/assets/favicon.ico" type="image/x-icon">
<link rel="icon" href="/assets/favicon.ico" type="image/x-icon">

<h2>Photos</h2>
<div class="photos-section">
  <ul>
    {% assign grouped_photos = site.photos | group_by: "category" | sort: "name" | reverse %}
    {% for year in grouped_photos %}
      <li class="category photos-year">
        {{ year.name }}
        <ul class="photos-months">
          {% assign sorted_photos = year.items | where: "type", "index" | sort: "date" | reverse %}
          {% for photo in sorted_photos %}
            <li class="photo-item">
              <a href="{{ photo.url }}" class="photo-link">
                {{ photo.title }}
              </a>
            </li>
          {% endfor %}
        </ul>
      </li>
    {% endfor %}
  </ul>
</div> 



<!-- Scroll track and bar -->
<div id="scrollTrack"></div>
<div id="verticalScrollProgress"></div>

<!-- Scroll hint 
<div id="scrollHint">▼ Scroll ▼</div>
<div id="scrollHintBottom">▼ Scroll ▼</div> -->


<style>
/* Scroll track (full height) */
#scrollTrack {
  position: fixed;
  top: 25%;            /* start 25% from top of page */
  left: 50%;
  transform: translateX(-700px);
  width: 5px;
  height: 50%;         /* cover middle 50% of the page */
  background-color: rgba(255, 255, 255, 0.1);
  z-index: 9998;
}

#verticalScrollProgress {
  position: fixed;
  top: 25%;            /* match track start */
  left: 50%;
  transform: translateX(-700px);
  width: 5px;
  height: 0%;          /* grows as you scroll */
  background-color: #5bff32;
  z-index: 9999;
}

/* Scroll hint text */
/*#scrollHint {
  position: fixed;
  top: 21%;          /* same top as track 
  left: 50%;
  transform: translateX(-700px); /* slightly left of the bar 
  color: #5bff32;
  font-family: monospace;
  font-size: 0.8em;
  z-index: 10000;
}


#scrollHintBottom {
  position: fixed;
  top: calc(27% + 50%); /* start top + track height 
  left: 50%;
  transform: translateX(-700px); /* same as top hint 
  color: #5bff32;
  font-family: monospace;
  font-size: 0.8em;
  z-index: 10000;
}
*/

/* Pulse animation */
@keyframes pulse {
  0% { opacity: 0.5; }
  50% { opacity: 1; }
  100% { opacity: 0.5; }
}

/* Center the entire page content */
/* Center the entire page content */
body {
  color: white;
  font-family: monospace;
  font-size: 1rem;
  line-height: 1.4;
  margin: 0;
  min-height: 100%;
  overflow-wrap: break-word;
  background-image: url('/assets/window.jpg'); 
  background-size: cover; 
  background-position: center; 
  background-attachment: fixed; 
  text-align: center;  /* centers top-level lists */
  font-weight: bold;   /* <-- make all text bold */
}

/* Center top-level lists while keeping bullets/indentation */
ul {
  display: inline-block;
  text-align: left;
  margin: 0 auto;       /* remove vertical margin */
  padding-left: 1.5em;
  list-style-position: outside;
  font-weight: bold;
}

/* Remove extra gap between headings and lists */
h2 + ul {
  margin-top: 0;
}

/* Nested lists preserve indentation and hierarchy */
ul ul, ul ul ul {
  display: block;
  margin-left: 0;
  padding-left: 1.2em;
  position: relative;   /* fixes dash alignment for posts */
  font-weight: bold;
}

/* Headings slightly more indented, left-aligned */
h2, h3 {
  text-align: left;
  padding-left: 4em;   
  margin-top: 1em;
  margin-bottom: 0.2em;
  font-weight: bold;
}
h1 {
  text-align: left;
  padding-left: 3em;  /* only h1 is indented 3em */
  margin-top: 1em;
  margin-bottom: 0.2em;
  font-weight: bold;
}


/* Category level */
.category {
  font-size: 1.2rem;
  font-weight: bold;
  list-style-type: disc;
  margin-top: 1em;  /* keep spacing between categories */
  font-weight: bold;
}

/* Remove top margin for the first category in each section */
ul > .category:first-child {
  margin-top: 0;
}

/* Extra space between Data Project categories */
h2 + ul .category {
  margin-top: 1.5em;
}

/* Year level */
.year {
  font-size: 1rem;
  font-weight: normal;
  margin-left: 3em;
  list-style-type: circle;
  font-weight: bold;
}

/* Posts level dashes */
ul ul ul {
  list-style-type: none;  /* remove squares */
  padding-left: 1.8em;
}

ul ul ul li {
  position: relative;    /* needed for pseudo-element */
}

ul ul ul li::before {
  content: "– ";         /* dash */
  color: rgba(255, 255, 255, 1);
  position: absolute;
  left: -1.2em;          /* aligns dash correctly at start */
  font-weight: bold;
}

/* Links */
a {
  color: rgb(91, 255, 50);
  text-decoration: underline;
  font-weight: bold;
}

.photos-section > ul {
  padding-left: 0; /* outer list aligned with other sections */
}

/* Year styling */
.photos-section .photos-year {
  margin-left: -8.4em; /* move year text */
  font-weight: bold;   /* make year text bold */
  font-size: 1.2rem;
  font-weight: bold;
}

/* Month links (post links) indentation */
.photos-section .photos-months {
  padding-left: 2em; /* adjust how far post links are from the year */
  list-style-type: none; /* remove default bullets */
  font-weight: bold;
}

/* Add dash before each post link */
.photos-section .photo-item::before {
  content: "– ";       /* dash */
  color: rgba(255, 255, 255, 1);
  font-weight: normal; /* force dash to stay normal */
  position: absolute;
  margin-left: -1.5em; /* positions the dash at start */
  font-weight: bold;
}

/* Style the links */
.photos-section .photo-link {
  font-size: 0.8em;
  font-weight: normal;  /* force link text normal */
  text-decoration: underline;
  color: rgb(91, 255, 50);
  position: relative; /* required for dash positioning */
  font-weight: bold;
}


</style>

<script>
window.onscroll = function() {
  const track = document.getElementById("scrollTrack");
  const bar = document.getElementById("verticalScrollProgress");

  const scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;

  // Track position and height
  const trackTop = track.offsetTop;
  const trackHeight = track.offsetHeight;

  // Map scroll progress to track height
  const scrolled = (scrollTop / scrollHeight) * trackHeight;
  bar.style.height = scrolled + "px";
};
</script>