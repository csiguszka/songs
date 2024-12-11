<template>
  <div id="app">
    <h1>Song List</h1>
    <div>
      <input v-model="searchTitle" placeholder="Search by title" />
      <input v-model="searchArtist" placeholder="Search by artist" />
      <select v-model="searchGenre">
        <option value="">All Genres</option>
        <option v-for="genre in uniqueGenres" :key="genre" :value="genre">{{ genre }}</option>
      </select>
    </div>
    <table>
      <thead>
        <tr>
          <th>Title</th>
          <th>Artist</th>
          <th>Genre</th>
          <th>Year</th>
          <th>Time</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="song in filteredSongs" :key="song._id">
          <td>{{ song.TITLE }}</td>
          <td>{{ getArtistName(song.ARTIST_ID) }}</td>
          <td>{{ song.GENRE }}</td>
          <td>{{ song.YEAR }}</td>
          <td>{{ song.TIME }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import songsData from './data/songs.js';
import artistData from './data/artists.js';

export default {
  data() {
    return {
      songs: songsData,
      artists: artistData,
      searchTitle: '',
      searchArtist: '',
      searchGenre: '',
    };
  },
  computed: {
    uniqueGenres() {
      return [...new Set(this.songs.map(song => song.GENRE))].filter(Boolean);
    },
    filteredSongs() {
      return this.songs.filter(song => {
        const artistName = this.getArtistName(song.ARTIST_ID).toLowerCase();
        return (
          (!this.searchTitle || song.TITLE.toLowerCase().includes(this.searchTitle.toLowerCase())) &&
          (!this.searchArtist || artistName.includes(this.searchArtist.toLowerCase())) &&
          (!this.searchGenre || song.GENRE === this.searchGenre)
        );
      });
    },
  },
  methods: {
    getArtistName(artistId) {
      const artist = this.artists.find(a => a._id === artistId);
      return artist ? artist.artist : 'Unknown';
    },
  },
};
</script>

<style>
body {
  font-family: 'Comic Sans MS', sans-serif;
  margin: 20px;
  background: linear-gradient(90deg, #f0f9ff, #cbebff);
  color: #333;
}
input, select {
  margin: 5px;
  padding: 10px;
  border: 1px solid #0073e6;
  border-radius: 5px;
  background-color: #f7faff;
  color: #0073e6;
}
table {
  width: 100vh;
  min-width: 100vh;
  border-collapse: collapse;
  margin-top: 20px;
  background-color: #f9fcff;
  border: 2px solid #0073e6;
}
th, td {
  border: 1px solid #0073e6;
  padding: 10px;
  text-align: left;
}
th {
  background-color: #0073e6;
  color: #fff;
}
tbody tr:nth-child(odd) {
  background-color: #e6f7ff;
}
tbody tr:nth-child(even) {
  background-color: #ffffff;
}
</style>
