<template>    
    <div>
        <b-navbar toggleable="md" type="light" variant="light">
            <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
            <b-collapse is-nav id="nav_collapse">
                
                <b-navbar-brand to="/"><i class="fas fa-shopping-cart"></i> changocart</b-navbar-brand>

                <b-navbar-nav>
                    <b-nav-item v-if="isAuthenticated" to="add-item">Add Item</b-nav-item>
                    <b-nav-item v-if="isAuthenticated" to="shop">Shop</b-nav-item>
                    <b-nav-item v-if="isAuthenticated" to="orders">Orders</b-nav-item>            
                </b-navbar-nav>

                <b-navbar-nav class="ml-auto">
                    <b-nav-item v-if="!isAuthenticated" to="login" right>Login</b-nav-item>    
                    <b-nav-item v-if="isAuthenticated" @click="logout" right>Logout</b-nav-item>
                </b-navbar-nav>

            </b-collapse>

            <b-navbar-nav class="ml-auto" v-b-popover.hover.top="'Checkout'">

                <b-nav-item type="light" to="cart" v-if="cartCount > 0">
                    <button id="basketButton"> 
                        <i class="basket fas fa-shopping-basket" right></i>
                        <span class="basketText" v-if="cartCount != 0"> {{cartCount}} </span>
                        </button>
                </b-nav-item>
                
            </b-navbar-nav>   
        </b-navbar>
    </div>
</template>

<script>
    import { mapGetters } from "vuex";
    
    export default {
        name: "navBar",

        methods: {
            logout() {
                this.$store.dispatch('logout');
            }
        },
        
        computed: mapGetters(['isAuthenticated', 'currentUser', 'cartCount']),            
    }
</script>

<style >
    .b-img {
        display: block;
        max-width: 100%;
        height: auto;
    }

    #basketButton {
        background: black;
        color: white;
        height: 30px;
        border-left-width: 2px ;
        border-right-width:2px ;
        border-top-width: 2px ; 
        border-bottom-width: 2px;
        border-color: black;
    }

    .basketText {
        color:white;
    }
</style>

