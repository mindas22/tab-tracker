<template>
  <v-layout>
    <v-flex xs4>
      <panel title="Song Metadata">
        <v-text-field
          label="Title"
          required
          :rules="[required]"
          v-model="song.title"
        ></v-text-field>

        <v-text-field
          label="Artist"
          required
          :rules="[required]"
          v-model="song.artist"
        ></v-text-field>

        <v-text-field
          label="Genre"
          required
          :rules="[required]"
          v-model="song.genre"
        ></v-text-field>

        <v-text-field
          label="Album"
          required
          :rules="[required]"
          v-model="song.album"
        ></v-text-field>

        <v-text-field
          label="Album Image URL"
          required
          :rules="[required]"
          v-model="song.albumImageUrl"
        ></v-text-field>

        <v-text-field
          label="Youtube ID"
          required
          :rules="[required]"
          v-model="song.youtubeId"
        ></v-text-field>

      </panel>
    </v-flex>
    <v-flex xs8 class="ml-2">
      <panel title="Song structure">
       <v-text-field
          label="Lyrics"
          required
          :rules="[required]"
          multi-line
          v-model="song.lyrics"
        ></v-text-field>

        <v-text-field
          label="TAB"
          required
          :rules="[required]"
          multi-line
          v-model="song.tab"
        ></v-text-field>
      </panel>

      <div class="danger-alert" v-if="error">
        {{error}}
      </div>
       <v-btn
        class="cyan" dark
        @click="create">
        Create Song
      </v-btn>
    </v-flex>
  </v-layout>
</template>

<script>
import Panel from '@/components/Panel'
import SongsService from '@/services/SongsService'

export default {
  data () {
    return {
      song: {
        title: null,
        artist: null,
        genre: null,
        album: null,
        albumImageUrl: null,
        youtubeId: null,
        lyrics: null,
        tab: null
      },
      error: null,
      required: (value) => !!value || 'Required.'
    }
  },
  methods: {
    async create () {
      // call API to create song
      this.error = null
      const areAllFieldsFiledIn = Object
        .keys(this.song)
        .every(key => !!this.song[key])

      if (!areAllFieldsFiledIn) {
        this.error = 'Please fill in all required fields.'
        return
      }
      try {
        await SongsService.post(this.song)
        this.$router.push({
          name: 'songs'
        })
      } catch (err) {
        console.log(err)
      }
    }
  },
  components: {
    Panel
  }
}
</script>

<style scoped>
</style>
