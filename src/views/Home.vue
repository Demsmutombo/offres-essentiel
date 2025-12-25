<template>
  <div class="index-page">
    <Header />
    
    <main class="main">
      <!-- Hero Section -->
      <section id="hero" class="hero section dark-background">
        <img :src="photoPrincipale || '/assets/img/IMG_20240915_050452_231.jpg'" alt="" data-aos="fade-in">
        <div class="container d-flex flex-column align-items-center justify-content-center text-center" data-aos="fade-up" data-aos-delay="100">
          <h2>En mémoire de {{ nomComplet }}</h2>
          <p v-if="surnoms">{{ surnoms }}</p>
        </div>
      </section>

      <!-- About Section - Informations personnelles + Biographie -->
      <section id="about" class="about section">
        <br><br><br>
        <div class="container" data-aos="fade-up" data-aos-delay="100">
          <div class="row gy-4">
            <div class="col-md-6">
              <div class="row justify-content-between gy-4">
                <div class="col-lg-5">
                  <img src="/assets/img/FB_IMG_1726419737492.jpg" class="img-fluid" alt="Photo principale">
                </div>
                <div class="col-lg-7 about-info">
                  <p><strong>Nom complet : </strong> <span>Henock Ngandu Kabadi</span></p>
                  <p v-if="surnoms"><strong>Surnom(s) : </strong> <span>{{ surnoms }}</span></p>
                  <p><strong>Date de naissance : </strong> <span>{{ formatDate(naissanceJour, naissanceMois, naissanceAnnee) }}</span></p>
                  <p><strong>Date de décès : </strong> <span>{{ formatDate(decesJour, decesMois, decesAnnee) }}</span></p>
                  <p><strong>Lieu de naissance : </strong> <span>{{ lieuNaissance }}</span></p>
                  <p><strong>Lieu de résidence : </strong> <span>{{ lieuResidence }}</span></p>
                  <p><strong>Profession : </strong> <span>{{ profession }}</span></p>
                </div>
              </div>
            </div>

            <div class="col-md-6">
              <div class="about-me">
                <h4>Biographie</h4>
                <p v-html="formatBiographie(biographie)"></p>
                
                <h5 style="margin-top: 20px; margin-bottom: 10px; color: var(--heading-color);">Famille</h5>
                <p v-if="pere || mere"><strong>Parents :</strong> 
                  <span v-if="pere">Père: {{ pere }}</span>
                  <span v-if="pere && mere">, </span>
                  <span v-if="mere">Mère: {{ mere }}</span>
                </p>
                <p v-if="positionFratrie"><strong>Frères et sœurs :</strong> {{ positionFratrie }}</p>
                <p v-if="nombreEnfants > 0 || nomsEnfants">
                  <strong>Enfants :</strong> 
                  <span v-if="nombreEnfants > 0">{{ nombreEnfants }} enfant(s)</span>
                  <span v-if="nombreEnfants > 0 && nomsEnfants"> - </span>
                  <span v-if="nomsEnfants">{{ nomsEnfants }}</span>
                </p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Obsèques Section -->
      <section id="funeral-info" class="section" style="background-color: #f9f9f9;">
        <div class="container" data-aos="fade-up">
          <div class="section-title">
            <h2>Informations sur les Obsèques</h2>
            <p>Détails des cérémonies et lieu de sépulture</p>
          </div>
          <div class="row gy-4">
            <div class="col-md-6" v-if="veilleeJour || veilleeLieu">
              <div class="info-item" style="padding: 25px; background: white; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1);">
                <h4 style="color: var(--accent-color); margin-bottom: 20px;"><i class="bi bi-calendar-event me-2"></i>Veillée Mortuaire</h4>
                <p v-if="veilleeJour"><strong>Date :</strong> {{ formatDate(veilleeJour, veilleeMois, veilleeAnnee) }}</p>
                <p v-if="veilleeHeure"><strong>Heure :</strong> {{ formatHeure(veilleeHeure) }}</p>
                <p v-if="veilleeLieu"><strong>Lieu :</strong> {{ veilleeLieu }}</p>
              </div>
            </div>
            <div class="col-md-6" v-if="ceremonieJour || ceremonieLieu">
              <div class="info-item" style="padding: 25px; background: white; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1);">
                <h4 style="color: var(--accent-color); margin-bottom: 20px;"><i class="bi bi-calendar-check me-2"></i>Cérémonie Funéraire</h4>
                <p v-if="ceremonieJour"><strong>Date :</strong> {{ formatDate(ceremonieJour, ceremonieMois, ceremonieAnnee) }}</p>
                <p v-if="ceremonieHeure"><strong>Heure :</strong> {{ formatHeure(ceremonieHeure) }}</p>
                <p v-if="ceremonieLieu"><strong>Lieu :</strong> {{ ceremonieLieu }}</p>
              </div>
            </div>
            <div class="col-md-12" v-if="cimetiere">
              <div class="info-item" style="padding: 25px; background: white; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1);">
                <h4 style="color: var(--accent-color); margin-bottom: 20px;"><i class="bi bi-geo-alt me-2"></i>Lieu de Sépulture</h4>
                <p><strong>Cimetière :</strong> {{ cimetiere }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Portfolio Section - Galerie photos (maximum 15 pour formule Essentiel) -->
      <section id="portfolio" class="portfolio section" v-if="allPhotos.length > 0">
        <div class="container section-title" data-aos="fade-up">
          <h2>Galerie</h2>
          <p>Retour en images</p>
        </div>

        <div class="container">
          <div class="row gy-4" data-aos="fade-up" data-aos-delay="200">
            <!-- Maximum 15 photos pour la formule Essentiel -->
            <div class="col-lg-4 col-md-6 portfolio-item" v-for="(img, index) in allPhotos.slice(0, 15)" :key="index">
              <a :href="img" data-lightbox="gallery" :data-title="`Photo ${index + 1}`">
                <img :src="img" class="img-fluid" :alt="`Photo ${index + 1}`">
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact Section -->
      <section id="contact" class="contact section">
        <div class="container section-title" data-aos="fade-up">
          <h2>Contact</h2>
          <p>Pour toute question ou information</p>
        </div>

        <div class="container" data-aos="fade-up" data-aos-delay="100">
          <div class="info-wrap" data-aos="fade-up" data-aos-delay="200">
            <div class="row gy-5">
              <div class="col-lg-6" v-if="contactTelephone">
                <div class="info-item d-flex align-items-center">
                  <i class="bi bi-telephone flex-shrink-0"></i>
                  <div>
                    <h3>Téléphone</h3>
                    <p><a :href="`tel:${contactTelephone.replace(/[^0-9+]/g, '')}`" style="color:green;">{{ contactTelephone }}</a></p>
                  </div>
                </div>
              </div>

              <div class="col-lg-6" v-if="contactWhatsApp">
                <div class="info-item d-flex align-items-center">
                  <i class="bi bi-whatsapp flex-shrink-0"></i>
                  <div>
                    <h3>WhatsApp</h3>
                    <p><a :href="`https://wa.me/${contactWhatsApp.replace(/[^0-9]/g, '')}`" target="_blank" style="color:green;">{{ contactWhatsApp }}</a></p>
                  </div>
                </div>
              </div>

              <div class="col-lg-6" v-if="contactEmail">
                <div class="info-item d-flex align-items-center">
                  <i class="bi bi-envelope flex-shrink-0"></i>
                  <div>
                    <h3>Email</h3>
                    <p><a :href="`mailto:${contactEmail}`" style="color:green;">{{ contactEmail }}</a></p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="text-center mt-4" v-if="contactNom">
            <p><strong>Personne contact :</strong> {{ contactNom }}</p>
          </div>
        </div>
      </section>
    </main>

    <Footer />

    <!-- Scroll Top -->
    <a href="#" id="scroll-top" class="scroll-top d-flex align-items-center justify-content-center" @click.prevent="scrollToTop"><i class="bi bi-arrow-up-short"></i></a>

    <!-- Preloader -->
    <div id="preloader"></div>
  </div>
</template>

<script>
import Header from '../components/Header.vue'
import Footer from '../components/Footer.vue'

export default {
  name: 'Home',
  components: {
    Header,
    Footer
  },
  data() {
    return {
      // Données du défunt - à remplir depuis le formulaire ou API
      nomComplet: 'Henock Ngandu Kabadi',
      surnoms: '',
      naissanceJour: 15,
      naissanceMois: 5,
      naissanceAnnee: 1992,
      decesJour: 2,
      decesMois: 9,
      decesAnnee: 2024,
      lieuNaissance: 'République Démocratique du Congo',
      lieuResidence: 'Afrique du Sud (Pretoria, puis Cape Town)',
      profession: 'Passionné de football',
      
      // Informations familiales
      pere: 'Famille Ngandu',
      mere: 'Famille Ngandu',
      nombreEnfants: 1,
      nomsEnfants: 'Karine Fumbi',
      positionFratrie: '5ème d\'une famille de 7 enfants',
      
      // Biographie (1-2 paragraphes, 200-300 mots)
      biographie: 'Henock Ngandu Kabadi est une figure notable du football, dont la passion pour le sport a marqué son enfance. Dès son jeune âge, il s\'est distingué comme un talent prometteur et a été désigné meilleur joueur au sein du COÉ. À l\'âge de 14 ans, il a été sélectionné pour un stage de perfectionnement en France, où son jeu a surpris les sélectionneurs, dont Claude Le Roy, alors entraîneur des Léopards de la RDC.\n\nIl a fait ses études primaires à l\'école catholique Sainte-Marie Goretti, obtenu son baccalauréat à la Télé de la Victoire, et poursuivi ses études universitaires à l\'Université Révérend Kim. Sa passion pour le football l\'a conduit à vivre d\'abord à Pretoria, puis à Cape Town, en Afrique du Sud.',
      
      // Obsèques
      veilleeJour: 8,
      veilleeMois: 9,
      veilleeAnnee: 2024,
      veilleeHeure: '18:00',
      veilleeLieu: 'Domicile familial',
      ceremonieJour: 9,
      ceremonieMois: 9,
      ceremonieAnnee: 2024,
      ceremonieHeure: '10:00',
      ceremonieLieu: 'Église',
      cimetiere: 'Nécropole de l\'Eternité',
      
      // Photos (maximum 15 pour Essentiel)
      allPhotos: [
        '/assets/img/FB_IMG_1726419798066.jpg',
        '/assets/img/FB_IMG_1726419737492.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0027.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0037.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0042.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0026.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0029.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0034.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0024.jpg',
        '/assets/img/portfolio/IMG-20240905-WA0036.jpg',
        '/assets/img/portfolio/cimetiere/IMG-20240919-WA0024.jpg',
        '/assets/img/portfolio/cimetiere/IMG-20240919-WA0022.jpg',
        '/assets/img/portfolio/cimetiere/IMG-20240919-WA0129.jpg',
        '/assets/img/portfolio/cimetiere/IMG-20240919-WA0126.jpg',
        '/assets/img/portfolio/cimetiere/IMG-20240919-WA0134.jpg'
      ],
      
      // Contact
      contactNom: '',
      contactTelephone: '+243 991 683 269',
      contactEmail: 'israelmutombo9319@gmail.com',
      contactWhatsApp: '+243 991 683 269'
    }
  },
  mounted() {
    this.initScripts()
    // Charger les données depuis localStorage ou API si disponibles
    this.loadData()
  },
  methods: {
    loadData() {
      // Charger les données depuis localStorage (si sauvegardées depuis le formulaire)
      const savedData = localStorage.getItem('memorialData')
      if (savedData) {
        try {
          const data = JSON.parse(savedData)
          Object.assign(this, data)
        } catch (e) {
          console.error('Erreur lors du chargement des données:', e)
        }
      }
    },
    formatDate(jour, mois, annee) {
      if (!jour || !mois || !annee) return ''
      const moisNoms = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 
                       'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre']
      return `${jour} ${moisNoms[mois - 1]} ${annee}`
    },
    formatHeure(heure) {
      if (!heure) return ''
      const [h, m] = heure.split(':')
      return `${h}h${m || '00'}`
    },
    formatBiographie(text) {
      if (!text) return ''
      // Convertir les sauts de ligne en paragraphes
      return text.split('\n\n').map(para => `<p>${para}</p>`).join('')
    },
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    },
    async initScripts() {
      // Load all vendor scripts
      const scripts = [
        '/assets/vendor/bootstrap/js/bootstrap.bundle.min.js',
        '/assets/vendor/aos/aos.js',
        '/assets/vendor/typed.js/typed.umd.js',
        '/assets/vendor/imagesloaded/imagesloaded.pkgd.min.js',
        '/assets/vendor/isotope-layout/isotope.pkgd.min.js',
        'https://cdnjs.cloudflare.com/ajax/libs/lightbox2/2.11.3/js/lightbox.min.js'
      ]

      try {
        await Promise.all(scripts.map(script => this.loadScript(script)))
        
        // Wait a bit for scripts to be fully available
        setTimeout(() => {
          this.initAOS()
          this.initTyped()
          this.initIsotope()
          this.initScrollTop()
          this.initPreloader()
          this.initScrolled()
          // Lightbox2 auto-initializes when it sees data-lightbox attributes
        }, 100)
      } catch (error) {
        console.error('Error loading scripts:', error)
      }
    },
    loadScript(src) {
      return new Promise((resolve, reject) => {
        if (document.querySelector(`script[src="${src}"]`)) {
          resolve()
          return
        }
        const script = document.createElement('script')
        script.src = src
        script.onload = resolve
        script.onerror = reject
        document.head.appendChild(script)
      })
    },
    initAOS() {
      if (window.AOS) {
        window.AOS.init({
          duration: 600,
          easing: 'ease-in-out',
          once: true,
          mirror: false
        })
      }
    },
    initTyped() {
      const selectTyped = document.querySelector('.typed')
      if (selectTyped && window.Typed) {
        const typed_strings = selectTyped.getAttribute('data-typed-items').split(',')
        new window.Typed('.typed', {
          strings: typed_strings,
          loop: true,
          typeSpeed: 100,
          backSpeed: 50,
          backDelay: 2000
        })
      }
    },
    initIsotope() {
      if (window.Isotope && window.imagesLoaded) {
        document.querySelectorAll('.isotope-layout').forEach((isotopeItem) => {
          const layout = isotopeItem.getAttribute('data-layout') ?? 'masonry'
          const filter = isotopeItem.getAttribute('data-default-filter') ?? '*'
          const sort = isotopeItem.getAttribute('data-sort') ?? 'original-order'

          let initIsotope
          window.imagesLoaded(isotopeItem.querySelector('.isotope-container'), () => {
            initIsotope = new window.Isotope(isotopeItem.querySelector('.isotope-container'), {
              itemSelector: '.isotope-item',
              layoutMode: layout,
              filter: filter,
              sortBy: sort
            })
          })
        })
      }
    },
    initScrollTop() {
      const scrollTop = document.querySelector('.scroll-top')
      if (scrollTop) {
        const toggleScrollTop = () => {
          window.scrollY > 100 ? scrollTop.classList.add('active') : scrollTop.classList.remove('active')
        }
        window.addEventListener('load', toggleScrollTop)
        window.addEventListener('scroll', toggleScrollTop)
      }
    },
    initPreloader() {
      const preloader = document.querySelector('#preloader')
      if (preloader) {
        window.addEventListener('load', () => {
          preloader.remove()
        })
        // Remove preloader if already loaded
        if (document.readyState === 'complete') {
          preloader.remove()
        }
      }
    },
    initScrolled() {
      const selectBody = document.querySelector('body')
      const selectHeader = document.querySelector('#header')
      if (!selectHeader) return
      
      const toggleScrolled = () => {
        if (!selectHeader.classList.contains('scroll-up-sticky') && 
            !selectHeader.classList.contains('sticky-top') && 
            !selectHeader.classList.contains('fixed-top')) return
        window.scrollY > 100 ? selectBody.classList.add('scrolled') : selectBody.classList.remove('scrolled')
      }
      
      window.addEventListener('scroll', toggleScrolled)
      toggleScrolled()
    }
  }
}
</script>

<style scoped>
/* Styles spécifiques pour la formule Essentiel */
.about-info p {
  margin-bottom: 10px;
  line-height: 1.8;
}

.about-me p {
  line-height: 1.8;
  margin-bottom: 15px;
}

.info-item {
  transition: transform 0.3s;
}

.info-item:hover {
  transform: translateY(-5px);
}

.portfolio-item {
  position: relative;
  overflow: hidden;
  border-radius: 8px;
}

.portfolio-item img {
  transition: transform 0.3s;
  width: 100%;
  height: 250px;
  object-fit: cover;
}

.portfolio-item:hover img {
  transform: scale(1.1);
}

.portfolio-item a {
  display: block;
  position: relative;
}

@media (max-width: 768px) {
  .about-info {
    margin-top: 20px;
  }
}
</style>

