<template>
  <div id="app">
    <div id="nav">
      <header class="ms-header ms-header-white">
        <!--ms-header-white-->
        <div class="container container-full">
          <div class="ms-title">
            <router-link to="/">
              <!-- <img src="assets/img/demo/logo-header.png" alt=""> -->
              <span class="ms-logo animated zoomInDown animation-delay-5">Q</span>
              <h1 class="animated fadeInRight animation-delay-6">
                Quill
                <span>Demo</span>
              </h1>
            </router-link>
          </div>
          <div class="header-right">
            <a
              href="javascript:void(0)"
              class="btn-circle btn-circle-primary no-focus animated zoomInDown animation-delay-8"
              data-toggle="modal"
              data-target="#ms-account-modal"
            >
              <i class="zmdi zmdi-account"></i>
            </a>
            <form class="search-form animated zoomInDown animation-delay-9">
              <input id="search-box" type="text" class="search-input" placeholder="Search..." name="q" />
              <label for="search-box"><i class="zmdi zmdi-search"></i></label>
            </form>
          </div>
        </div>
      </header>
    </div>
    <router-view />
  </div>
</template>

<style></style>
