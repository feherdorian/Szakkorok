<template>
  <div class="szakkorok-container">
    <div class="gyerekek-lista box">
      <h3 class="title">Gyerekek</h3>
      <table>
        <thead>
          <tr>
            <th>Név</th>
            <th>Osztály</th>
            <th>Szakkör</th>
          </tr>
        </thead>
        <tbody>
          <tr 
            v-for="gyerek in gyerekek" 
            :key="gyerek.id"
            @click="selectGyerek(gyerek)"
            :class="{ selected: gyerek.id === selectedGyerek?.id }">
            <td>{{ gyerek.nev }}</td>
            <td>{{ gyerek.osztaly }}</td>
            <td>
              <select v-model="gyerek.selectedSzakkor" @change="updateSzakkor(gyerek)" class="szakkor-select">
                <option value="">Válassz szakkört</option>
                <option 
                  v-for="szakkor in szakkorok" 
                  :key="szakkor.id" 
                  :value="szakkor.id">
                  {{ szakkor.nev }}
                </option>
              </select>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="separator"></div> <!-- Elválasztó vonal -->

    <div class="szakkorok-lista box col-md-7">
      <h3 class="title">Szakkörök</h3>
      <div class="szakkorok-grid">
        <!-- Szakkörök listája -->
        <div 
          v-for="szakkor in szakkorok" 
          :key="szakkor.id" 
          class="szakkor-box">
          <span class="szakkor-nev">{{ szakkor.nev }}</span> ({{ szakkor.gyerekek.length }} tag)
          <ul>
            <!-- Gyerekek listája, akik részt vesznek a szakkörben -->
            <li v-for="gyerekId in szakkor.gyerekek" :key="gyerekId">
              {{ gyerekek.find(gyerek => gyerek.id === gyerekId).nev }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Gyerekek adatai
      gyerekek: [
        { id: 1, nev: 'Kiss Anna', osztaly: '5A', selectedSzakkor: '' },
        { id: 2, nev: 'Nagy Béla', osztaly: '6B', selectedSzakkor: '' },
        { id: 3, nev: 'Tóth Gábor', osztaly: '4C', selectedSzakkor: '' },
        { id: 4, nev: 'Szabó László', osztaly: '5B', selectedSzakkor: '' },
        { id: 5, nev: 'Varga Emília', osztaly: '6C', selectedSzakkor: '' },
        { id: 6, nev: 'Kovács Péter', osztaly: '4A', selectedSzakkor: '' },
        { id: 7, nev: 'Kiss Zita', osztaly: '5A', selectedSzakkor: '' },
        { id: 8, nev: 'Nagy Kálmán', osztaly: '6B', selectedSzakkor: '' },
        { id: 9, nev: 'Tóth Éva', osztaly: '4C', selectedSzakkor: '' },
        { id: 10, nev: 'Kiss András', osztaly: '5B', selectedSzakkor: '' },
      ],
      // Szakkörök adatai
      szakkorok: [
        { id: 1, nev: 'Kosárlabda', gyerekek: [] },
        { id: 2, nev: 'Rajz', gyerekek: [] },
        { id: 3, nev: 'Foci', gyerekek: [] },
        { id: 4, nev: 'Zene', gyerekek: [] },
        { id: 5, nev: 'Tánc', gyerekek: [] },
        { id: 6, nev: 'Programozás', gyerekek: [] },
      ],
      // Kiválasztott gyerek
      selectedGyerek: null,
    };
  },
  methods: {
    // Kiválasztott gyerek beállítása
    selectGyerek(gyerek) {
      this.selectedGyerek = gyerek;
    },
    // Szakkör frissítése
    updateSzakkor(gyerek) {
      // Először távolítsuk el a gyereket a korábbi szakkörből, ha van
      this.szakkorok.forEach(szakkor => {
        const gyerekIndex = szakkor.gyerekek.indexOf(gyerek.id);
        if (gyerekIndex !== -1) {
          szakkor.gyerekek.splice(gyerekIndex, 1);
        }
      });

      // Ha a gyereknek van kiválasztott szakköre, akkor adjuk hozzá
      if (gyerek.selectedSzakkor) {
        const szakkor = this.szakkorok.find(szakkor => szakkor.id === gyerek.selectedSzakkor);
        if (szakkor) {
          szakkor.gyerekek.push(gyerek.id);
        }
      }
    },
  },
};
</script>

<style scoped>
.szakkorok-container {
  display: flex; /* Flexbox elrendezés a gyerekek számára */
  justify-content: space-between; /* Egyenlő távolság a dobozok között */
  padding: 20px; /* Belső margó */
  align-items: flex-start; /* Felső igazítás */
  background-color: var(--bg-black-100); /* Háttér szín */
  flex-wrap: wrap; /* A tartalom több sorba kerül, ha nem fér el egy sorban */
}

.box {
  width: 45%; /* A doboz szélessége */
  padding: 20px; /* Belső margó */
  border: 1px solid rgba(255, 255, 255, 0.2); /* Határvonal */
  border-radius: 8px; /* Lekerekített sarkok */
  background-color: var(--bg-black-50); /* Háttér szín a dobozokban */
  box-shadow: 0 4px 20px var(--text-color); /* Világítás / árnyék hatás */
  transition: transform 0.3s, box-shadow 0.3s; /* Animáció az átalakulásokhoz */
  margin-bottom: 20px; /* Alsó margó, hogy a dobozok ne érjenek össze kis képernyőkön */
}

.box:hover {
  transform: translateY(-3px); /* Felugrásos effektus hover esetén */
  box-shadow: 0 8px 40px var(--text-color); /* Tovább erősített árnyék hover esetén */
}

.separator {
  width: 2px; /* Elválasztó vonal szélessége */
  height: auto; /* Magasság a szülő elemetől függ */
  background-color: white; /* Elválasztó vonal színe */
  margin: 0 20px; /* Külső margó */
  align-self: stretch; /* Kitölti a rendelkezésre álló magasságot */
}

table {
  width: 100%; /* Táblázat teljes szélessége */
  border-collapse: collapse; /* Határvonalak összeolvadása */
}

th, td {
  border: 1px solid #ddd; /* Határvonal a cellák között */
  padding: 8px; /* Cellák belső margója */
  text-align: left; /* Balra igazítás */
}

th {
  background-color: var(--text-color); /* Cím szín */
  color: white; /* Cím szín */
}

tr:hover {
  background-color: var(--text-color); /* Hover hatás a sorok számára */
}

.selected {
  background-color: var(--text-color); /* Kiemelt háttérszín a kiválasztott soroknak */
  color: white; /* Kiemelt szín */
}

.szakkorok-grid {
  display: grid; /* Rács elrendezés */
  grid-template-columns: repeat(2, 1fr); /* Alapértelmezett 2 oszlopos elrendezés */
  gap: 20px; /* Két doboz közötti távolság */
}

.szakkor-box {
  padding: 15px; /* Belső margó a dobozokban */
  border: 1px solid rgba(255, 255, 255, 0.2); /* Határvonal */
  border-radius: 8px; /* Lekerekített sarkok */
  background-color: var(--bg-black-50); /* Háttér szín a dobozokban */
  box-shadow: 0 4px 20px var(--text-color); /* Világítás / árnyék hatás */
  transition: transform 0.3s, box-shadow 0.3s; /* Animáció az átalakulásokhoz */

  /* Max magasság beállítása és görgetés engedélyezése a túltöltött tartalomhoz */
  max-height: 300px; /* Állítsd be ezt a magasságot szükség szerint */
  overflow-y: auto; /* Engedélyezd a függőleges görgetést, ha a tartalom túllépi a max magasságot */
}

.szakkor-box:hover {
  transform: translateY(-3px); /* Felugrásos effektus hover esetén */
  box-shadow: 0 8px 40px var(--text-color); /* Tovább erősített árnyék hover esetén */
}

.szakkor-select {
  padding: 5px; /* Belső margó a legördülő listánál */
  border: 1px solid rgba(255, 255, 255, 0.2); /* Határvonal */
  border-radius: 4px; /* Lekerekített sarkok */
  background-color: var(--bg-black-50); /* Háttér szín */
  color: white; /* Szöveg szín */
}

.szakkor-select option {
  background-color: var(--bg-black-50); /* Háttér szín a lehetőségekhez */
  color: white; /* Szöveg szín */
}

/* Reszponzív megjelenítés kis képernyőkön */
@media (max-width: 768px) {
  .szakkorok-container {
    flex-direction: column; /* Kis képernyőkön egymás alá kerülnek az elemek */
  }

  .box {
    width: 100%; /* A dobozok teljes szélességűek lesznek kis képernyőkön */
  }

  .szakkorok-grid {
    grid-template-columns: 1fr; /* Egyszerűsítjük a rácsot egy oszlopos elrendezésre */
  }

  .separator {
    display: none; /* Kis képernyőkön eltüntetjük az elválasztó vonalat */
  }
}

@media (max-width: 480px) {
  .box {
    padding: 10px; /* Kis képernyőkön csökkentsük a belső margókat */
  }

  th, td {
    padding: 5px; /* Csökkentsük a cellák belső margóját */
  }
}
</style>

