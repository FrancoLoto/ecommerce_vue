<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="clumn is-12">
                <h1 class="title">Página de pago</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Producto</th>
                            <th>Precio</th>
                            <th>Cantidad</th>
                            <th>Total</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                        >
                            <td>{{ item.product.name }}</td>
                            <td>${{ item.product.price }}</td>
                            <td>{{ item.quantity }}</td>
                            <td>${{ getItemTotal(item).toFixed(2) }}</td>
                        </tr> 
                    </tbody>

                    <tfoot>
                        <tr>
                            <td colspan="2">Total</td>
                            <td>{{ cartTotalLength }}</td>
                            <td>${{ cartTotalPrice.toFixed(2) }}</td>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Detalles de envío</h2>

                <p class="has-text-grey mb-4">* Todos los campos son requeridos</p>
            

                <div class="columns is-multiline">
                    <div class="column is-6">
                        <div class="field">
                            <label>Nombre *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="first_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>Apellido *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="last_name">
                            </div>
                        </div>

                        <div class="field">
                            <label>Email *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="email">
                            </div>
                        </div>

                        <div class="field">
                            <label>Teléfono *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="phone">
                            </div>
                        </div>
                    </div>

                    <div class="column is-6">
                        <div class="field">
                            <label>Dirección *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address">
                            </div>
                        </div>

                        <div class="field">
                            <label>Código postal *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="zipcode">
                            </div>
                        </div>

                        <div class="field">
                            <label>Ciudad *</label>
                            <div class="control">
                                <input type="text" class="input" v-model="place">
                            </div>
                        </div>
                    </div>

                    <div class="notification is-danger mt-4" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <hr>

                    <div id="card-element" class="mb-5"></div>

                    <template v-if="cartTotalLength">
                        <hr>

                        <button class="button is-dark" @click="submitForm">Pagar</button>
                    </template>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Checkout',
    data() {
        return {
            cart: {
                items: []
            },
            stripe: {},
            card: {},
            first_name: '',
            last_name: '',
            email: '',
            phone: '',
            address: '',
            zipcode: '',
            place: '',
            errors: []
        }
    },
    mounted() {
        document.title = 'Página de pago | Fibra positiva'

        this.cart = this.$store.state.cart
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        submitForm() {
            this.errors = []

            if (this.first_name === '') {
                this.errors.push('Debes agregar un nombre.')
            }

            if (this.last_name === '') {
                this.errors.push('Debes agregar un apellido.')
            }

            if (this.email === '') {
                this.errors.push('Debes agregar un email.')
            }

            if (this.phone === '') {
                this.errors.push('Debes agregar un teléfono.')
            }

            if (this.address === '') {
                this.errors.push('Debes agregar una dirección.')
            }

            if (this.zipcode === '') {
                this.errors.push('Debes agregar un código postal.')
            }

            if (this.place === '') {
                this.errors.push('Debes agregar una ciudad.')
            }
        }
    },
    computed: {
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        }
    }
}
</script>