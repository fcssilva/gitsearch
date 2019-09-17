<template>
    <section class="search">
        <div class="container col">
            <h1 class="search__title">
                Digite o nome do usuário do Git-Hub
            </h1>
            
            <form class="search__form">
                <input type="text" class="search__input" :value="user">
                <button class="search__btn" @click.prevent="searchUsersList">Buscar</button>
            </form>
        </div>
    </section>
</template>

<script>
export default {
    props: {
        user: String,
        searchUsersList: Function
    },
    name: 'Search',
}
</script>

<style scoped>
.col {
    flex-direction: column;
}

.search {
    margin-bottom: 40px;
    text-align: center;
    padding: 20px;
}

.search__form {
    display: flex;
}

.search__input {
    height: 20px;
    width: 80%;
    padding: 10px 15px;
    font-size: 1.8rem;
    color: #380021;
    outline: none;
    line-height: 1.5;
    border: 1px solid #380021;
}

.search__btn {
    background-color: #48335d;
    padding: 10px 15px;
    font-size: 1.1rem;
    font-weight: 600;
    color: #fff;
    outline: none;
    border: 0.5px solid #48335d;
    box-shadow: none;
    cursor: pointer;
}
</style>