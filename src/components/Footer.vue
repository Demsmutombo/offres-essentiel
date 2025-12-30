<template>
  <footer id="footer" class="footer accent-background">
    <div class="container">
      <div class="copyright text-center">
        <img src="/assets/img/log.png" alt="IM DIGITAL LABS" style="max-width: 60px; height: auto; margin-bottom: 8px;">
        <p style="margin-bottom: 8px; font-size: 14px;">© <span>Copyright</span> <strong class="px-1 sitename"><span style="color: #d4af37;">IM DIGITAL LABS</span></strong> <span>All Rights Reserved</span></p>
      </div>
      <div class="credits text-center" style="margin-top: 0;">
        <p style="font-size: 13px; margin-bottom: 0;">Conçu par <strong style="color: #d4af37;">IM DIGITAL LABS</strong> | 
          <a href="https://wa.me/243991683269" target="_blank" style="color: #25D366; text-decoration: none;">+243 991 683 269</a> | 
          <a href="mailto:imdigitallabs@gmail.com" style="color: inherit; text-decoration: none;">imdigitallabs@gmail.com</a>
        </p>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style scoped>
#footer {
  padding: 15px 0 !important;
}

#footer .container {
  padding: 0 15px;
}

#footer .copyright {
  margin-bottom: 5px;
}

#footer .credits {
  margin-top: 0;
}
</style>

