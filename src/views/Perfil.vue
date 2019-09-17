<template>
    <div class="perfil">
        <Header />
        <section class="user-info">  
            <div class="container row">
                <img class="user-info__logo" :src="userInfos.avatar_url">
                
                <div class="user-info__data col">
                    
                    <h1 class="user-info__title">
                        {{ userInfos.login }}
                    </h1>
                    <p class="user-info__text" v-if="userInfos.email">
                        {{ userInfos.email }}
                    </p>
                    <p class="user-info__text" v-if="userInfos.bio">
                        {{ userInfos.bio }}
                    </p>

                    <div class="user-info__follows">
                        <p class="user-info__text">
                            Seguidores: {{ userInfos.followers }}
                        </p>
                        <p class="user-info__text">
                            Seguindo: {{ userInfos.following }}
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <section class="repositories" v-if="userRepos.length">
            <div class="container">
                <h1 class="repositories__title">
                    Repositórios
                </h1>

                <div class="repositories__results">
                    <p>
                        Exibindo {{ userRepos.length }} repositórios
                    </p>
                </div>

                <ul class="repo-list">
                    <li class="repo-list__items"
                        v-for="(repo, index) in userRepos" 
                        :key="index" 
                        :class="`repo-list__element-${index}`"
                    >
                        <div class="repo-list__head">
                            <h2 class="repo-list__text--white">
                                {{ repo.name }}
                            </h2>
                            <p class="repo-list__text repo-list__text--white">
                                <i class="far fa-star repo-list__text--white"></i> 
                                {{ repo.stargazers_count }}
                            </p>
                        </div>

                        <p class="repo-list__text">
                            {{ repo.description }}
                        </p>

                        <div class="repo-list__options">
                            <span class="repo-list__link" @click="goToRepositorie(repo.name)">Ver detalhes</span>
                        </div>
                    </li>
                </ul>
            </div>
        </section>

        <section class="repo-list" v-else>
            <div class="container">
                <h2 class="repo-list__subtitle">
                    Nenhum repositório encontrado para o usuário {{ userInfos.login }}
                </h2>
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
        id: String 
    },
    data() {
        return {
            userInfos: [],
            userRepos: []
        }
    },
    mounted() {
        this.userInfo = []

        if(!this.id.length)
            return

        // Buscar dados do usuário
        axios
            .get(`https://api.github.com/users/${this.id}`)
            .then(response => this.userInfos = response.data)
            .catch(error=>{
                console.log(error)
            })

        axios
            .get(`https://api.github.com/users/${ this.id }/repos`)
            .then(response => this.sortByStars(response.data))
            .catch(error => {
                console.log(error)
            })
    },
    methods: {
        sortByStars(json) {
            this.userRepos = json.sort((a, b) => b.stargazers_count - a.stargazers_count)
        },
        goToRepositorie(repoName) {
            this.$router.push({ path: `/repositorie/${ this.id }/${ repoName }` })
        }
    }
}
</script>

<style>
.user-info {
    background-color: #f9f9f9;
}

.user-info__data {
    max-width: 50%;
    padding: 10px;
    justify-content: space-between;
}

.user-info__logo {
    height: 150px;
    width: 150px;
    padding: 30px;
    padding-left: 0;
}

.user-info__title {
    background-color: #48335d;
    padding: 10px;
    color: #fff;
}

.user-info__title,
.user-info__text {
    margin: 10px 0;
}

.user-info__follows {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    max-width: 300px;
    margin: 0px;
    padding: 0px;
}

.repositories__results {
    text-align: center;
}

.repo-list {
    list-style: none;
    width: 100%;
    margin: 0px;
    padding: 0px;
}

.repo-list__text {
    margin: 10px;
    word-wrap: break-word;
}

.repo-list__text--white {
    color: #fff;
    width: 100%;
    word-wrap: break-word;
}

.repo-list__head {
    display: flex;
    flex-direction: column-reverse;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding: 0 10px;
    color: #FFF;
    background-color: #48335d;
}

.repo-list__items {
    padding: 10px;
    margin: 20px 10px 0;
    box-shadow: 0 1px 5px #48335d;
}

.repo-list__title {
    font-size: 2rem;
    margin-bottom: 10px;
    word-wrap: break-word;
}

.repo-list__subtitle {
    margin-bottom: 10px;
    font-size: 1.2rem;
}

.repo-list__options {
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
    margin-top: 20px;
}

.repo-list__link {
    padding: 10px 20px;
    margin: 1px;
    background-color: #48335d;
    color: #FFF;
    text-decoration: none;
    cursor: pointer;
}


@media (max-width: 360px) {
    .user-info__data { align-items: center; text-align: center}
    .user-info__follows {justify-content: center}
    .user-info__logo { padding: 30px }
    .repo-list__head { flex-direction: column-reverse; justify-content: center; }
    .repo-list__options { flex-direction: column; align-items: center; text-align: center }
    .repo-list__link { width: calc(100% - 40px) }
}

/* @media (min-width: 361px) {
    .container { max-width: 100% }
    .repo-list { display: flex; flex-direction: row; flex-wrap: wrap; width: 100%}
    .repo-list__items { width: calc(50% - 60px) }
} */
</style>