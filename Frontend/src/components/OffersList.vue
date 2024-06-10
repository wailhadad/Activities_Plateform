<template>
  <div class="offers-list">
    <h3>Toutes les Offres</h3>
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Rechercher des offres..." @input="searchOffers" />
    </div>
    <div v-if="loading" class="loader">Chargement des offres...</div>
    <div v-else>
      <div v-if="error" class="error-message">
        <p>Une erreur s'est produite lors du chargement des offres : {{ error }}</p>
        <button @click="fetchOffers">Réessayer</button>
      </div>
      <div v-else class="offers-grid">
        <div class="offer" v-for="offer in filteredOffers" :key="offer.id">
          <img src="@/assets/child.png" alt="Offer Image" class="offer-image">
          <div class="offer-details">
            <h4>{{ offer.titre }}</h4>
            <p>{{ offer.description.slice(0, 100) }}...</p>
            <router-link :to="{ name: 'offerdetails', params: { id: offer.id }}" class="details-btn">
              Voir Détails
            </router-link>
          </div>
        </div>
      </div>
      <div class="pagination">
        <button @click="prevPage" :disabled="currentPage === 1">Précédent</button>
        <span>Page {{ currentPage }} sur {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Suivant</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '@/axios'; // Ensure this path is correct based on your project structure

export default {
  name: 'OffersList',
  data() {
    return {
      offers: [],
      loading: true,
      error: null,
      searchQuery: '',
      currentPage: 1,
      itemsPerPage: 8, // Changed to display 8 offers per page
    }
  },
  computed: {
    filteredOffers() {
      const searchLower = this.searchQuery.toLowerCase();
      return this.offers.filter(offer =>
          offer.titre.toLowerCase().includes(searchLower) ||
          offer.description.toLowerCase().includes(searchLower)
      ).slice(this.startIndex, this.endIndex);
    },
    startIndex() {
      return (this.currentPage - 1) * this.itemsPerPage;
    },
    endIndex() {
      return this.currentPage * this.itemsPerPage;
    },
    totalPages() {
      return Math.ceil(this.offers.length / this.itemsPerPage);
    }
  },
  created() {
    this.fetchOffers();
  },
  methods: {
    fetchOffers() {
      this.loading = true;
      this.error = null;
      axios.get('/show/offers')
          .then(response => {
            this.offers = response.data;
            this.loading = false;
          })
          .catch(error => {
            console.error('There was an error fetching the offers:', error);
            this.error = 'Impossible de charger les offres. Veuillez réessayer plus tard.';
            this.loading = false;
          });
    },
    searchOffers() {
      this.currentPage = 1;
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    }
  }
}
</script>
