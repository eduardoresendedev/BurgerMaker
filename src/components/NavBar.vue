<script>
export default {
    name: "NavBar",
    props: ["logo","alt"]
}
</script>

<template>
    <div id="nav">    
        <RouterLink to="/" id="logo_url">
            <img :src="logo" :alt="alt" id="logo">
        </RouterLink>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/pedidos">Pedidos</RouterLink>
    </div>
</template>


<style scoped>


#nav {
    top: 0;
    background-color: #222;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    position: fixed;
    width: 100%;
}

#nav #logo_url {
    margin: auto;
    margin-left: 0;

}

#logo {
    width: 40px;
    height: 40px;
}

#nav a {
    color: #fcba03;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
}

#nav a:hover{
    color: #fff;
}

</style>