<template>
  <div class="container mx-auto">
    <div class="w-full shadow-xl max-w-3xl mx-auto">
      <form
        class="bg-white px-8 pt-6 pb-8 mb-4 rounded"
        @submit.prevent="fetchTag"
      >
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
            for="username"
          >
            URL
          </label>
          <input
            id="url"
            v-model="form.url"
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            type="text"
            placeholder="URL"
          />
        </div>
        <div class="mb-6 mt-12">
          <p class="block text-gray-700 text-sm font-bold ">タグ一覧</p>
          <div class="w-full bg-500-red mt-4 flex p-tags flex-wrap shadow p-2">
            <div
              v-for="(item, index) in tagList"
              :key="index"
              class="inline-block bg-gray-200 border rounded-full m-2 px-3 py-1 text-sm font-semibold text-gray-700 mr-2"
            >
              {{ item }}
              <a
                href="#"
                class="rounded-full bg-gray-600 w-5  text-center text-white text-xs inline-block"
                @click.prevent="removeTag(index)"
              >
                X
              </a>
            </div>
          </div>
          <a
            href="#"
            class="bg-blue-500 select-none inline-block mt-8 hover:bg-blue-400  hover:shadow-lg shadow duration-300 text-white ease-in-out font-bold py-2 px-4 rounded"
            :class="buttonClass"
            @click.prevent="copyTags"
            >コピーする</a
          >
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tagList: [],
      form: {
        url: null
      },
      reg: /youtu(?:.*\/v\/|.*v=|\.be\/)([A-Za-z0-9_-]{11})/
    }
  },
  computed: {
    buttonClass() {
      return {
        'p-button__disabled': this.tagList.length <= 0
      }
    }
  },
  methods: {
    async fetchTag() {
      const id = this.isValidUrl()
      if (id) {
        const result = await this.$axios.get(
          `https://www.googleapis.com/youtube/v3/videos?key=${process.env.API_KEY}&fields=items(snippet(tags))&part=snippet&id=${id}`
        )
        this.tagList = result.data.items[0]?.snippet.tags
      } else {
        alert('有効なURLを入力してください!!')
        this.form.url = ''
      }
    },

    isValidUrl() {
      const match = this.form.url.match(this.reg)
      const id = match?.[1]
      return id
    },

    removeTag(index) {
      this.tagList.splice(index, 1)
    },

    copyTags() {
      const dummy = document.createElement('textarea')
      document.body.appendChild(dummy)
      dummy.value = this.tagList.join(',')
      dummy.select()
      document.execCommand('copy')
      document.body.removeChild(dummy)
      alert('タグをコピーしました！')
    }
  }
}
</script>

<style>
.p-tags {
  min-height: 5rem;
}

.p-button__disabled {
  pointer-events: none;
  opacity: 0.5;
}
</style>
