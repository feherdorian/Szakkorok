<template>
  <div class="szakkorok-container">
    <div class="gyerekek-lista box">
      <h3 class="title">Gyerekek</h3>
      <ul>
        <!-- Gyerekek listája, amely a gyerekek tömbből generálódik -->
        <li 
          v-for="gyerek in gyerekek" 
          :key="gyerek.id" 
          @click="selectGyerek(gyerek)" 
          :class="{ selected: gyerek.id === selectedGyerek?.id }">
          {{ gyerek.nev }} ({{ gyerek.osztaly }})
        </li>
      </ul>
    </div>

    <!-- Elválasztó vonal -->
    <div class="separator"></div>

    <div class="szakkorok-lista box">
      <h3 class="title">{{ selectedGyerek ? selectedGyerek.nev + ' szakkörei:' : 'Szakkörök' }}</h3>
      <div v-if="selectedGyerek">
        <ul>
          <!-- Szakkörök listája a kiválasztott gyerek alapján -->
          <li v-for="szakkor in szakkorok" :key="szakkor.id">
            <span class="szakkor-nev">{{ szakkor.nev }} (Tagok: {{ szakkor.tagokSzama }})</span>
            <input type="checkbox" v-model="szakkor.kivalasztva" @change="toggleSzakkor(szakkor)" />
          </li>
        </ul>
      </div>
      <div v-else>
        <!-- Üzenet, ha nincs kiválasztva gyerek -->
        <p class="message">Kérlek, válassz egy gyereket!</p>
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
        { id: 1, nev: 'Kiss Anna', osztaly: '5A' },
        { id: 2, nev: 'Nagy Béla', osztaly: '6B' },
        { id: 3, nev: 'Tóth Gábor', osztaly: '4C' },
        { id: 4, nev: 'Szabó László', osztaly: '5B' },
        { id: 5, nev: 'Varga Emília', osztaly: '6C' },
        { id: 6, nev: 'Kovács Péter', osztaly: '4A' },
        { id: 7, nev: 'Kiss Zita', osztaly: '5A' },
        { id: 8, nev: 'Nagy Kálmán', osztaly: '6B' },
        { id: 9, nev: 'Tóth Éva', osztaly: '4C' },
        { id: 10, nev: 'Kiss András', osztaly: '5B' },
      ],
      // Szakkörök adatai
      szakkorok: [
        { id: 1, nev: 'Kosárlabda', tagokSzama: 10, gyerekek: [1, 2], kivalasztva: false },
        { id: 2, nev: 'Rajz', tagokSzama: 5, gyerekek: [3], kivalasztva: false },
        { id: 3, nev: 'Foci', tagokSzama: 12, gyerekek: [1], kivalasztva: false },
        { id: 4, nev: 'Zene', tagokSzama: 8, gyerekek: [], kivalasztva: false },
        { id: 5, nev: 'Tánc', tagokSzama: 7, gyerekek: [], kivalasztva: false },
        { id: 6, nev: 'Programozás', tagokSzama: 4, gyerekek: [], kivalasztva: false },
      ],
      // Kiválasztott gyerek
      selectedGyerek: null,
    };
  },
  methods: {
    // Kiválasztott gyerek beállítása
    selectGyerek(gyerek) {
      this.selectedGyerek = gyerek;
      this.szakkorok.forEach(szakkor => {
        szakkor.kivalasztva = szakkor.gyerekek.includes(gyerek.id);
      });
    },
    // Szakkörök kiválasztásának váltása
    toggleSzakkor(szakkor) {
      if (szakkor.kivalasztva) {
        // Ha kiválasztva van, és a gyerek nincs benne, hozzáadjuk
        if (!szakkor.gyerekek.includes(this.selectedGyerek.id)) {
          szakkor.gyerekek.push(this.selectedGyerek.id);
          szakkor.tagokSzama++;
        }
      } else {
        // Ha nem kiválasztva van, eltávolítjuk a gyereket
        const index = szakkor.gyerekek.indexOf(this.selectedGyerek.id);
        if (index > -1) {
          szakkor.gyerekek.splice(index, 1);
          szakkor.tagokSzama--;
        }
      }
    },
  },
};
</script>

<style scoped>
.szakkorok-container {
  display: flex; /* Flexbox elrendezés a gyerekek és szakkörök számára */
  justify-content: space-between; /* Egyenlő távolság a dobozok között */
  padding: 20px; /* Belső margó */
  align-items: flex-start; /* Felső igazítás */
  background-color: var(--bg-black-50); /* Háttér szín */
}

.box {
  width: 45%; /* A doboz szélessége */
  padding: 20px; /* Belső margó */
  border: 1px solid rgba(255, 255, 255, 0.2); /* Határvonal */
  border-radius: 8px; /* Lekerekített sarkok */
  background-color: var(--bg-black-50); /* Háttér szín a dobozokban */
  box-shadow: 0 4px 20px var(--text-color); /* Árnyék hatás */
  transition: transform 0.3s, box-shadow 0.3s; /* Animáció az átalakulásokhoz */
}

.box:hover {
  transform: translateY(-3px); /* Felugrásos effektus hover esetén */
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.3); /* Tovább erősített árnyék hover esetén */
}

.separator {
  width: 2px; /* Elválasztó vonal szélessége */
  background-color: black; /* Elválasztó vonal színe */
  height: 100%; /* Elválasztó vonal magassága */
  margin: 0 20px; /* Külső margó */
}

ul {
  list-style-type: none; /* Lista pontok eltüntetése */
  padding: 0; /* Lista belső margó eltüntetése */
}

li {
  padding: 10px; /* Listaelem belső margó */
  cursor: pointer; /* Kéz kurzor a listaelem fölött */
  border-bottom: 1px solid #ddd; /* Listaelem alsó határvonal */
  transition: background-color 0.2s; /* Animáció a háttérszín változásához */
}

li:hover {
  background-color: var(--text-color); /* Hover hatás a háttérszínre */
}

li:last-child {
  border-bottom: none; /* Utolsó listaelem alsó határvonalának eltüntetése */
}

li.selected {
  font-weight: bold; /* Kiemelt szöveg vastagsága */
  background-color: var(--text-color); /* Kiválasztott gyerek háttérszín */
  border-left: 4px solid var(--bg-black-50); /* Kiválasztott gyerek stílus */
}

input[type="checkbox"] {
  margin-left: 10px; /* Checkbox bal margó */
}

.title {
  color: var(--text-color); /* Szín a címeknek */
  margin-bottom: 15px; /* Cím alatti margó */
}

.message {
  color: var(--text-color); /* Szín a szövegeknek */
  margin: 10px 0; /* Szöveg körüli margó */
}

.szakkor-nev {
  font-weight: bold; /* Kiemelt szakkör neve vastag */
  color: white; /* Kiemelt szakkör neve színe */
}
</style>
