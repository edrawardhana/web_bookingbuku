<template>
  <div class="about">
    <div class="about__content">
      <!-- Added a container for the content -->
      <h1 class="about__title">About Source Web Lending Library</h1>
      <p>
        Welcome to the Source Web Lending Library, your one-stop shop for all
        your web development learning needs! We offer a vast collection of
        resources, from beginner-friendly tutorials to advanced guides on
        cutting-edge technologies.
      </p>

      <p>
        Our mission is to empower developers of all levels with the knowledge
        and tools they need to succeed in the ever-evolving world of web
        development. Whether you're just starting out or looking to expand your
        expertise, we have something for you.
      </p>

      <h2 class="about__subtitle">What We Offer:</h2>
      <ul class="about__list">
        <li>
          Comprehensive Documentation: Explore detailed guides and tutorials
          covering a wide range of web technologies.
        </li>
        <li>
          Interactive Code Examples: Learn by doing with hands-on coding
          exercises and examples.
        </li>
        <li>
          Community Forum: Connect with fellow developers, ask questions, and
          share your knowledge.
        </li>
        <li>
          Expert Support: Get assistance from our experienced team of web
          development professionals.
        </li>
      </ul>

      <p>Join our community today and start building amazing things!</p>
    </div>
  </div>
</template>

<style scoped>
.about {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 20px; /* add padding to the section*/
  text-align: left; /* Align the text to the left within the content area */
}

.about__content {
  /* Styles for the content container*/
  background-color: white;
  max-width: 800px; /* set a maximum container width */
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Add a subtle shadow */
}

.about__title {
  font-size: 2.5rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
}

.about__subtitle {
  font-size: 1.8rem;
  color: #333;
  margin-top: 30px; /* Add spacing above the subtitle */
  margin-bottom: 15px; /* Add spacing below the subtitle */
}

.about__list {
  list-style-type: disc; /* Use bullet points */
  padding-left: 40px; /* Indent the list items */
  line-height: 1.6; /* Increase line spacing */
}
</style>
