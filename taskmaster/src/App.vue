<template>
  <div class="app-container">
    <header class="navbar">
      <h1>CSC 4370 | Home Work 3</h1>
      <nav>
        <RouterLink to="/">Dashboard</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </header>

    <main class="content">
      <RouterView />
    </main>
  </div>
</template>

<style>
/* Global Styles */
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f3f4f6;
  margin: 0;
  color: #1f2937;
}

.app-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #2563eb;
  color: white;
  padding: 15px 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.navbar h1 {
  margin: 0;
  font-size: 1.5rem;
}

.navbar nav {
  display: flex;
  gap: 15px;
}

.navbar a {
  color: white;
  text-decoration: none;
  font-weight: bold;
}

.navbar a:hover {
  text-decoration: underline;
}
</style>