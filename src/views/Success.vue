<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Gracias por tu compra!</h1>

                <p>Su orden será procesada dentro de las próximas 48hs.</p>
            </div>
        </div>
    </div>

</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Gracias por tu compra | Fibra Positiva'
    },
}
</script>