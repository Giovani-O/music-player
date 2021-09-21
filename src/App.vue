<template>
  <div id="app">
    <header>
      <h1 id="logo"> Psychi</h1>
    </header>
    <main>
      <section class="player">
        <img :src="current.imgSrc" alt="thumbnail" class="thumbnail">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
      </section>

      <section class="playlist">
        <h3>Playlist</h3>
        <div
          v-for="song in songs" 
          :key="song.src" 
        >
          <button 
            @click="play(song)"
            :class="(song.src == current.src) ? 'song playing' : 'song'"
          >
            {{ song.title }} - {{ song.artist }}
          </button>
          <br>
        </div>
      </section>
    </main>

    <footer>
      <span 
        :class="isPlaying ? 'song-tag scroll' : 'song-tag'"
      >
        {{ current.title }} - {{ current.artist }}
      </span><br>
      <div class="controls">
        <button
          class="volume decrease"
          @click="player.volume -= 0.1"
        >-</button>
        <button 
          class="prev"
          @click="prev"
        >Prev</button>
        <button 
          class="play" 
          @click="play"
          v-if="!isPlaying"
        >Play</button>
        <button 
          class="pause" 
          @click="pause()"
          v-else
        >Pause</button>
        <button 
          class="next"
          @click="next"
        >Next</button>
        <button
          class="volume increase"
          @click="player.volume += 0.1"
        >+</button>
      </div>
    </footer>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Neon Renegade',
          artist: 'Harris Heller',
          src: require('./assets/neonrenegade.mp3'),
          imgSrc: require('./assets/neonrenegade.jpg')
        },
        {
          title: 'Because The Night',
          artist: 'Harris Heller',
          src: require('./assets/becausethenight.mp3'),
          imgSrc: require('./assets/neonrenegade.jpg')
        },
        {
          title: 'Banhammer',
          artist: 'Harris Heller',
          src: require('./assets/banhammer.mp3'),
          imgSrc: require('./assets/neonrenegade.jpg')
        },
        {
          title: 'Wolf Princess',
          artist: 'Adrian Von Ziegler',
          src: require('./assets/wolfprincess.mp3'),
          imgSrc: require('./assets/celtic.jpg')
        },
      ],
      player: new Audio(),
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined"){
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
    },
    pause() {
        this.player.pause();
        this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0){
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.volume = 0.1;
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Galada&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
  background-color: #141414;
  color: #fff;
}
header {
  position: fixed;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 8px;
  background-color: #181818;
  color: #fff;
  margin-bottom: 10px;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
}

#logo {
  font-family: 'Galada', cursive;
}

.player {
  padding: 5px;
  margin-bottom: 5px;
  text-align: center;
}

.thumbnail {
  margin-top: 85px;
  width: 300px;
  height: 300px;
}

.song-title {
  font-family: 'Ubuntu', sans-serif;
  font-weight: 400;
}

.playlist {
  margin-top: 20px;
  margin-bottom: 90px;
  text-align: center;
  justify-content: center;
  font-family: 'Ubuntu', sans-serif;
  font-weight: 400;
}

.playlist h3 {
  padding: 5px;
}
.playlist button {
  padding: 10px;
  font-family: 'Ubuntu', sans-serif;
  font-weight: 300;
}

.controls {
  width: 100%;
  text-align: center;
  justify-content: center;
}

.song-tag {
  margin-bottom: 5px;
  white-space: nowrap;
  color: rgb(126, 57, 255);
}

.scroll {
  animation: text-scroll 10s linear infinite;
}

.prev, .next, .play, .pause, .volume {
  padding: 7px 15px;
  margin: 2px 5px;
  border: solid .2px rgb(43, 43, 43);
  border-radius: 8px;
  box-shadow: 0px 0px 6px .5px rgba(0, 0, 0, 0.5);
  background-color: #141414;
  color: #fff;
}

.increase {
  justify-self: end;
}
.decrease {
  justify-self: start;
}

.song {
  width: 97%;
  padding: 7px 15px;
  margin: 2px 5px;
  border: solid .2px rgb(43, 43, 43);
  border-radius: 8px;
  box-shadow: 0px 0px 6px .5px rgba(0, 0, 0, 0.5);
  background-color: #141414;
  color: #fff;
}

.playing {
  border: solid .2px rgb(89, 0, 255);
}

footer {
  position: fixed;
  bottom: 0;
  display: grid;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 10px 15px 15px 15px;
  background-color: #181818;
  color: #fff;
  width: 100%;
}

@keyframes text-scroll {
  0% {
    transform: translate(100%, 0);
  }
  50% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(-100%, 0);
  }
}
</style>
