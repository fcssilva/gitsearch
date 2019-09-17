<template>
  <div class="repositories">
    <Header />
    <section class="user-info">  
        <div class="container row">
            <div class="user-info__data col">
                
                <h1 class="user-info__title">
                    {{ repoInfos.name }}
                </h1>
                <p class="user-info__text" v-if="repoInfos.description">
                    {{ repoInfos.description }}
                </p>
                <p class="user-info__text" v-if="repoInfos.language">
                    {{ repoInfos.language }}
                </p>

                <div class="user-info__follows">
                    <a target="_blank" :href="repoInfos.html_url" class="user-info__text">
                        Acessar repositório
                    </a>
                </div>
            </div>
        </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header'

export default {
  components: {
    Header
  },
  props: {
    id: String,
    repo: String
  },
  data() {
    return {
      repoInfos: []
    }
  },
  mounted() {
    console.log(this.id, this.repo)
    let repoAux = []
    axios
      .get(`https://api.github.com/users/${ this.id }/repos`)
      .then(response => repoAux = response.data)
      .catch(error => {
          console.log(error)
      })
      .finally(() => {
        repoAux.forEach(repository => {
          if (repository.name == this.repo) {
            this.repoInfos = repository
          }
        })
      })
  }
}
</script>

<style>

</style>