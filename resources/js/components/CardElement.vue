<template>
    <div>
        <card class='stripe-card'
          :class='{ complete }'
          stripe='pk_test_Pob3hbobh05isJlsBZ4JcPRH'
          :options='stripeOptions'
          @change='change($event)'
        />
        <div id="card-errors" role="alert" v-text="errorMessage"/>
    </div>
</template>

<script>
    import { Card, createToken } from 'vue-stripe-elements-plus'

    export default {
        components: { Card },

        data () {
            return {
                complete: false,
                errorMessage: '',
                stripeOptions: {
                // see https://stripe.com/docs/stripe.js#element-options for details
                    style: {

                        base: {
                            color: 'black',
                            // lineHeight: '18px',
                            fontFamily: '"Raleway", Helvetica, sans-serif',
                            fontSmoothing: 'antialiased',
                            fontSize: '16px',
                            '::placeholder': {
                            color: '#aab7c4'
                            }
                        },
                        invalid: {
                            color: '#fa755a',
                            iconColor: '#fa755a'
                        }
                    },
                    hidePostalCode: true
                }
            }
        },
        methods: {
            pay () {
              // createToken returns a Promise which resolves in a result object with
              // either a token or an error key.
              // See https://stripe.com/docs/api#tokens for the token object.
              // See https://stripe.com/docs/api#errors for the error object.
              // More general https://stripe.com/docs/stripe.js#stripe-create-token.
              createToken().then(data => console.log(data.token))
            },

            change(event) {
                if (event.error) {
                  this.errorMessage = event.error.message;
                } else {
                  this.errorMessage = 'There is an Error'
                }

                this.errorMessage = event.error ? event.error.message : ''
            }
        } 
    }
</script>

<style>
    .StripeElement {
        box-sizing: border-box;

        height: 40px;

        padding: 10px 12px;

        border: 1px solid #ccd0d2;
        border-radius: 4px;
        background-color: white;

        box-shadow: 0 1px 3px 0 #e6ebf1;
        -webkit-transition: box-shadow 150ms ease;
        transition: box-shadow 150ms ease;
    }

    .StripeElement--focus {
        box-shadow: 0 1px 3px 0 #cfd7df;
    }

    .StripeElement--invalid {
        border-color: #fa755a;
    }

    .StripeElement--webkit-autofill {
        background-color: #fefde5 !important;
    }
</style>
