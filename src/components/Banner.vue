<template>
    <div id="main-banner">
        <h1> Pastelaria</h1>
    </div>
</template>

<script>
    export default{
        name: "Banner"
    }
</script>

<style scoped>

#main-banner{
    background-image: url(https://s2.glbimg.com/md4D16uQ0vBVD8u8agv0GwGB8oE=/940x523/e.glbimg.com/og/ed/f/original/2017/10/09/pastel.jpg);
    background-position: 0 -250px;
    background-size: cover;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
}

#main-banner h1{
    color: #fff;
    text-align: center;
    font-size: 60px;
    background-color: rgb(155, 28, 28);
    padding: 20px 40px;
    border: 4px solid #fcba03;
}
</style>
