<template>
	<nav>
    <div class="nav-wrapper">
      <a href="#" class="brand-logo">Logo</a>
      <ul id="nav-mobile" class="right hide-on-med-and-down">
        <router-link
					to="/"
					active-class="active"
					exact
					tag="li"> 
					Create
				</router-link>
        <router-link
					to="/list"
					active-class="active"
					tag="li"> 
					List
				</router-link>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {

}
</script>

<style scoped>
	.nav-wrapper{
		padding: 0 50px;
	}
	li{
		width: 100px;
		text-align: center;
	}
</style>