<template>
  <ul class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion" id="accordionSidebar">
    <a class="sidebar-brand d-flex align-items-center justify-content-center" href="index.html">
        <div class="sidebar-brand-icon rotate-n-15"><i class="fas fa-laugh-wink"></i></div>
        <div class="sidebar-brand-text mx-3">Technical Test PT KAM</div>
    </a>
    <hr class="sidebar-divider my-0" />
    <li class="nav-item"><NuxtLink to="/"><a class="nav-link"><i class="fas fa-fw fa-tachometer-alt"></i><span>Dashboard</span></a></NuxtLink></li>
    <hr class="sidebar-divider" />
    <div class="sidebar-heading">MENU</div>
    <li class="nav-item"><NuxtLink to="/list-order"><a class="nav-link"><i class="fas fa-fw fa-chart-area"></i><span>Order</span></a></NuxtLink></li>
    <hr class="sidebar-divider d-none d-md-block" />
    <div class="text-center d-none d-md-inline"><button class="rounded-circle border-0" id="sidebarToggle"></button></div>
  </ul>
</template>
