<template>
  <div class="country-app">
    <h1 class="app-title">Country Information</h1>
    <div class="search-container">
      <input v-model="searchQuery" type="text" placeholder="Find Country" @keyup.enter="searchCountries" />
      <button @click="searchCountries">Find</button>
    </div>
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    <div v-if="loading" class="loading-container">
      <div class="loading-spinner"></div>
    </div>
    <div class="countries-container">
      <div v-for="country in countries" :key="country.alpha3Code" class="country-card">
        <div class="country-details">
          <p><strong>Capital:</strong> {{ formatArray(country.capital) }}</p>
          <p><strong>Populations:</strong> {{ formatNumber(country.population) }}</p>
          <p><strong>Languages:</strong> {{ formatLanguages(country.languages) }}</p>
          <p><strong>Currency:</strong> {{ formatCurrencies(country.currencies) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      countries: [],
      errorMessage: '',
      loading: false
    };
  },
  methods: {
    async searchCountries() {
      if (this.searchQuery === '') {
        this.countries = [];
        this.errorMessage = 'Nama negara tidak boleh kosong';
        return;
      }

      try {
        this.loading = true; // Menampilkan animasi loading
        const apiUrl = `https://restcountries.com/v3.1/name/${this.searchQuery}`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        if (response.ok) {
          this.countries = data;
          this.errorMessage = '';
        } else {
          this.countries = [];
          this.errorMessage = 'Negara tidak ditemukan';
        }
      } catch (error) {
        console.error('Error searching countries:', error);
        this.countries = [];
        this.errorMessage = 'Terjadi kesalahan dalam mencari negara';
      } finally {
        this.loading = false; // Menyembunyikan animasi loading
      }
    },
    formatArray(array) {
      if (Array.isArray(array)) {
        return array.join(', ');
      }
      return array;
    },
    formatNumber(number) {
      return number ? number.toLocaleString() : '';
    },
    formatLanguages(languages) {
      if (languages && typeof languages === 'object') {
        const languageNames = Object.values(languages);
        return languageNames.join(', ');
      }
      return '';
    },
    formatCurrencies(currencies) {
      if (currencies && typeof currencies === 'object') {
        const currencyNames = Object.values(currencies).map(currency => currency.name);
        return currencyNames.join(', ');
      }
      return '';
    }
  }
};
</script>

<style scoped>
.country-app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.app-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.search-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input {
  width: 250px;
  padding: 8px;
  font-size: 16px;
  border-radius: 4px 0 0 4px;
  border: none;
}

button {
  padding: 8px 16px;
  font-size: 16px;
  border-radius: 0 4px 4px 0;
  border: none;
  background-color: #2196f3;
  color: #fff;
  cursor: pointer;
}

.error-message {
  color: red;
  margin-top: 10px;
}

.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.loading-spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #2196f3;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.countries-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-gap: 20px;
}

.country-card {
  background-color: #f3f3f3;
  padding: 20px;
  border-radius: 4px;
  display: flex;
  align-items: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.country-details {
  margin-left: 20px;
}

.country-name {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}
</style>
