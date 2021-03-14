<template>
  <div id="app">
    <nav class="navbar navbar-expand-md navbar-dark bg-dark navbar-fixed-top">
      <div class="container">
        <router-link to="/" class="navbar-brand">匯率站</router-link>
        <button class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li>
              <router-link
                class="nav-link"
                to="/Calculator">
                <i class="fa fa-calculator"></i>
                貨幣計算器
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <router-view/>
  </div>
</template>
<style lang='scss'>
@import './assets/scss/all';
</style>
