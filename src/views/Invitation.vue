<template>
  <div class="index-page">
    <Header />
    
    <main class="main">
      <!-- Carte Section -->
      <section id="carte" class="invitation-section">
        <div class="invitation-container">
          <!-- Cloud Background -->
          <div class="cloud-background"></div>
          
          <!-- Border -->
          <div class="invitation-border"></div>
          
          <!-- Content -->
          <div class="invitation-content">
            <!-- Top Text -->
            <h2 class="invitation-top-text">REPOSE EN PAIX</h2>
            
            <!-- Doves -->
            <div class="dove dove-left">
              <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
                <!-- Body -->
                <ellipse cx="100" cy="100" rx="35" ry="25" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Head -->
                <circle cx="100" cy="75" r="18" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Wing -->
                <path d="M 85 95 Q 70 85 65 100 Q 70 115 85 105 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <path d="M 115 95 Q 130 85 135 100 Q 130 115 115 105 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Tail -->
                <path d="M 100 120 Q 85 140 75 135 Q 85 130 100 125 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <path d="M 100 120 Q 115 140 125 135 Q 115 130 100 125 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Eye -->
                <circle cx="95" cy="72" r="3" fill="#333"/>
                <!-- Beak -->
                <path d="M 100 80 L 105 85 L 100 88 Z" fill="#ffa500"/>
              </svg>
            </div>
            <div class="dove dove-right">
              <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
                <!-- Body -->
                <ellipse cx="100" cy="100" rx="35" ry="25" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Head -->
                <circle cx="100" cy="75" r="18" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Wing -->
                <path d="M 85 95 Q 70 85 65 100 Q 70 115 85 105 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <path d="M 115 95 Q 130 85 135 100 Q 130 115 115 105 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Tail -->
                <path d="M 100 120 Q 85 140 75 135 Q 85 130 100 125 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <path d="M 100 120 Q 115 140 125 135 Q 115 130 100 125 Z" fill="white" stroke="#e0e0e0" stroke-width="1"/>
                <!-- Eye -->
                <circle cx="105" cy="72" r="3" fill="#333"/>
                <!-- Beak -->
                <path d="M 100 80 L 95 85 L 100 88 Z" fill="#ffa500"/>
              </svg>
            </div>
            
            <!-- Circular Portrait -->
            <div class="portrait-container">
              <img :src="portraitImage || '/assets/img/FB_IMG_1726419737492.jpg'" 
                   :alt="deceasedName" 
                   class="circular-portrait">
            </div>
            
            <!-- Name -->
            <h1 class="deceased-name">{{ deceasedName }}</h1>
            
            <!-- Dates -->
            <p class="dates">{{ formattedDates }}</p>
            
            <!-- Message 1 -->
            <p class="invitation-message">{{ message1 }}</p>
            
            <!-- Message 2 -->
            <p class="invitation-message-second">{{ message2 }}</p>
            
            <!-- Service Date and Time (optionnel) -->
            <p v-if="serviceDate" class="service-datetime">{{ serviceDate }} à {{ serviceTime }}</p>
            
            <!-- Venue (optionnel) -->
            <div v-if="venueName" class="venue-info">
              <p class="venue-name">{{ venueName }}</p>
              <p v-if="venueAddress" class="venue-address">{{ venueAddress }}</p>
            </div>
          </div>
        </div>
      </section>
      
      <Footer />
    </main>
  </div>
</template>

<script>
import Header from '../components/Header.vue'
import Footer from '../components/Footer.vue'

export default {
  name: 'Invitation',
  components: {
    Header,
    Footer
  },
  data() {
    return {
      // Informations du défunt
      deceasedName: 'Henock Ngandu Kabadi',
      formattedDates: '15 Mai 1992 - 2 Septembre 2024',
      portraitImage: '/assets/img/FB_IMG_1726419737492.jpg',
      
      // Messages
      message1: 'Que ton âme trouve la paix éternelle et le réconfort.',
      message2: 'Repose en paix, cher Henock. Tu nous manqueras profondément.',
      
      // Informations du service (optionnelles)
      serviceDate: '',
      serviceTime: '',
      
      // Lieu (optionnel)
      venueName: '',
      venueAddress: ''
    }
  },
  mounted() {
    // Charger les données depuis localStorage ou API si disponibles
    this.loadData()
  },
  methods: {
    loadData() {
      // Charger depuis localStorage si disponible (même source que Home.vue)
      const savedData = localStorage.getItem('memorialData')
      if (savedData) {
        try {
          const data = JSON.parse(savedData)
          // Mettre à jour les données de la carte avec les données du mémorial
          if (data.nomComplet) this.deceasedName = data.nomComplet
          if (data.naissanceJour && data.naissanceMois && data.naissanceAnnee && data.decesJour && data.decesMois && data.decesAnnee) {
            this.formattedDates = this.formatFrenchDates(data.naissanceJour, data.naissanceMois, data.naissanceAnnee, data.decesJour, data.decesMois, data.decesAnnee)
          }
          if (data.photoPrincipale) this.portraitImage = data.photoPrincipale
          if (data.ceremonieJour && data.ceremonieMois && data.ceremonieAnnee) {
            this.serviceDate = this.formatFrenchServiceDate(data.ceremonieJour, data.ceremonieMois, data.ceremonieAnnee)
          }
          if (data.ceremonieHeure) {
            this.serviceTime = this.formatFrenchTime(data.ceremonieHeure)
          }
          if (data.ceremonieLieu) this.venueName = data.ceremonieLieu
          if (data.cimetiere) {
            this.venueAddress = `Cimetière : ${data.cimetiere}`
          }
        } catch (e) {
          console.error('Error loading carte data:', e)
        }
      }
    },
    formatFrenchDates(jourNaiss, moisNaiss, anneeNaiss, jourDeces, moisDeces, anneeDeces) {
      const moisNoms = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin',
                       'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre']
      const dateNaiss = `${jourNaiss} ${moisNoms[moisNaiss - 1]} ${anneeNaiss}`
      const dateDeces = `${jourDeces} ${moisNoms[moisDeces - 1]} ${anneeDeces}`
      return `${dateNaiss} - ${dateDeces}`
    },
    formatFrenchServiceDate(jour, mois, annee) {
      const date = new Date(annee, mois - 1, jour)
      const joursSemaine = ['Dimanche', 'Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi']
      const moisNoms = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin',
                       'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre']
      return `${joursSemaine[date.getDay()]}, ${jour} ${moisNoms[mois - 1]} ${annee}`
    },
    formatFrenchTime(heure) {
      if (!heure) return ''
      const [h, m] = heure.split(':')
      return `${h}h${m || '00'}`
    }
  }
}
</script>

<style scoped>
.invitation-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 60px 20px;
  background: linear-gradient(to bottom, #e8f4f8 0%, #f5f5f5 100%);
  position: relative;
}

.invitation-container {
  position: relative;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  background: #ffffff;
  border-radius: 8px;
  padding: 60px 40px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.cloud-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    radial-gradient(ellipse at 20% 30%, rgba(255, 255, 255, 0.9) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 70%, rgba(240, 240, 240, 0.8) 0%, transparent 50%),
    radial-gradient(ellipse at 50% 50%, rgba(250, 250, 250, 0.7) 0%, transparent 60%);
  z-index: 0;
}

.invitation-border {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border: 2px solid #b3d9f2;
  border-radius: 8px;
  z-index: 1;
  pointer-events: none;
}

.invitation-content {
  position: relative;
  z-index: 2;
  text-align: center;
}

.invitation-top-text {
  font-family: 'Playfair Display', 'Georgia', serif;
  font-style: normal;
  font-size: 28px;
  color: #1a1a1a;
  margin-bottom: 40px;
  font-weight: 500;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.dove {
  position: absolute;
  width: 100px;
  height: 100px;
  z-index: 1;
  opacity: 0.85;
  pointer-events: none;
}

.dove-left {
  top: 12%;
  left: 8%;
  transform: rotate(-20deg) translateY(-10px);
}

.dove-right {
  bottom: 18%;
  right: 8%;
  transform: rotate(20deg) translateY(10px);
}

.dove svg {
  width: 100%;
  height: 100%;
  filter: drop-shadow(0 3px 6px rgba(0, 0, 0, 0.15));
}

.portrait-container {
  margin: 30px auto 40px;
  width: 220px;
  height: 220px;
  position: relative;
  z-index: 2;
}

.circular-portrait {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  border: 6px solid #ffffff;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  display: block;
}

.deceased-name {
  font-family: 'Playfair Display', 'Georgia', serif;
  font-size: 48px;
  font-weight: 400;
  color: #1a1a1a;
  margin: 20px 0 15px;
  letter-spacing: 2px;
  font-style: italic;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.05);
}

.dates {
  font-family: 'Roboto', 'Arial', sans-serif;
  font-size: 15px;
  color: #666;
  margin-bottom: 50px;
  letter-spacing: 1px;
  font-weight: 400;
}

.invitation-message {
  font-family: 'Roboto', 'Arial', sans-serif;
  font-size: 15px;
  color: #333;
  line-height: 1.8;
  margin: 30px auto 20px;
  max-width: 600px;
  letter-spacing: 0.5px;
  font-weight: 300;
  font-style: italic;
}

.invitation-message-second {
  font-family: 'Roboto', 'Arial', sans-serif;
  font-size: 14px;
  color: #555;
  line-height: 1.8;
  margin: 0 auto 40px;
  max-width: 600px;
  letter-spacing: 0.5px;
  font-weight: 300;
}

.service-datetime {
  font-family: 'Roboto', 'Arial', sans-serif;
  font-size: 16px;
  color: #1a1a1a;
  margin-bottom: 40px;
  letter-spacing: 1px;
  font-weight: 400;
  text-transform: uppercase;
}

.venue-info {
  margin-top: 40px;
}

.venue-name {
  font-family: 'Playfair Display', 'Georgia', serif;
  font-size: 24px;
  font-style: italic;
  color: #1a1a1a;
  margin-bottom: 10px;
  font-weight: 400;
}

.venue-address {
  font-family: 'Roboto', 'Arial', sans-serif;
  font-size: 14px;
  color: #666;
  letter-spacing: 0.5px;
  font-weight: 300;
  line-height: 1.6;
}

/* Responsive Design */
@media (max-width: 768px) {
  .invitation-container {
    padding: 40px 20px;
    margin: 20px;
  }
  
  .invitation-top-text {
    font-size: 20px;
    margin-bottom: 30px;
  }
  
  .portrait-container {
    width: 180px;
    height: 180px;
    margin: 20px auto 30px;
  }
  
  .deceased-name {
    font-size: 36px;
  }
  
  .dates {
    font-size: 12px;
    margin-bottom: 40px;
  }
  
  .invitation-message {
    font-size: 12px;
    margin-bottom: 25px;
  }
  
  .service-datetime {
    font-size: 14px;
    margin-bottom: 30px;
  }
  
  .venue-name {
    font-size: 20px;
  }
  
  .venue-address {
    font-size: 12px;
  }
  
  .dove {
    width: 60px;
    height: 60px;
    opacity: 0.7;
  }
  
  .dove-left {
    top: 10%;
    left: 5%;
  }
  
  .dove-right {
    bottom: 15%;
    right: 5%;
  }
}

@media (max-width: 480px) {
  .invitation-container {
    padding: 30px 15px;
  }
  
  .deceased-name {
    font-size: 28px;
  }
  
  .invitation-top-text {
    font-size: 18px;
  }
  
  .dove {
    display: none;
  }
}
</style>

