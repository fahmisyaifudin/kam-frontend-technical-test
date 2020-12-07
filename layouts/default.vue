<template>
  <div>
    <div>
        <div id="wrapper">
            <my-sidebar></my-sidebar>
            <div class="d-flex flex-column" id="content-wrapper">
                <div id="content">
                    <my-header></my-header>
                     <nuxt></nuxt>
                </div>
                <my-footer></my-footer>
            </div>
        </div><a class="scroll-to-top rounded" href="#page-top"><i class="fas fa-angle-up"></i></a>
        <div class="modal fade" id="logoutModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">Ready to Leave?</h5><button class="close" type="button" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button></div>
                    <div class="modal-body">Select &quot;Logout&quot; below if you are ready to end your current session.
                    </div>
                    <div class="modal-footer"><button class="btn btn-secondary" type="button" data-dismiss="modal">Cancel</button><a class="btn btn-primary" href="login.html">Logout</a></div>
                </div>
            </div>
        </div>
    </div>
   
  </div>
</template>

<script>
/*eslint-disable */
import MySidebar from "@@/components/ui/Sidebar"
import MyHeader from "@@/components/ui/Header"
import MyFooter from "@@/components/ui/Footer"

export default {
  components: { MySidebar, MyHeader, MyFooter },
  mounted() {
    !(function(t) {
      'use strict'
      t('#sidebarToggle, #sidebarToggleTop').on('click', function(o) {
        t('body').toggleClass('sidebar-toggled'),
          t('.sidebar').toggleClass('toggled'),
          t('.sidebar').hasClass('toggled') &&
            t('.sidebar .collapse').collapse('hide')
      }),
        t(window).resize(function() {
          t(window).width() < 768 && t('.sidebar .collapse').collapse('hide')
        }),
        t('body.fixed-nav .sidebar').on(
          'mousewheel DOMMouseScroll wheel',
          function(o) {
            if (768 < t(window).width()) {
              var e = o.originalEvent,
                l = e.wheelDelta || -e.detail
              ;(this.scrollTop += 30 * (l < 0 ? 1 : -1)), o.preventDefault()
            }
          }
        ),
        t(document).on('scroll', function() {
          100 < t(this).scrollTop()
            ? t('.scroll-to-top').fadeIn()
            : t('.scroll-to-top').fadeOut()
        }),
        t(document).on('click', 'a.scroll-to-top', function(o) {
          var e = t(this)
          t('html, body')
            .stop()
            .animate(
              { scrollTop: t(e.attr('href')).offset().top },
              1e3,
              'easeInOutExpo'
            ),
            o.preventDefault()
        })
    })(jQuery)
  }
}
</script>
