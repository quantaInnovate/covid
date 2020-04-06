<template>
  <div class="sticky-top">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="/">Not logo</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item to="/">Home</b-nav-item>
          <b-nav-item href="/hospital">Hospital</b-nav-item>
          <b-nav-item href="/case">Case</b-nav-item>
          <b-nav-item href="/ref">Data Refferrence</b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {}
</script>
