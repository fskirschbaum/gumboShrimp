<template>
    <b-container>
        <b-form class="addItem" >
            
            <b-form-group
                label-cols-lg="3"
                label="Add an Item"
                label-size="lg"
                label-class="font-weight-bold pt-0"
                class="mb-0">
            </b-form-group>       

            <b-form-group id="imageGroup" label="Image" label-for="image">
                    <b-form-file
                    id="image"
                    accept="image/*"
                    v-model="form.itemImage"
                    placeholder="Choose an image..."
                    @change="onImageChange"/>
                
                <b-col cols="6" style="margin-top: 1rem;">
                    <img v-if="form.url" :src="form.url" width="420" alt="uploaded image">
                </b-col>
                <b-form-invalid-feedback id="imageLiveFeedback">
                    Please enter an image
                </b-form-invalid-feedback>
            </b-form-group> 
            
            <b-form-group id="itemInputGroup" label="Item Name" label-for="item">
                <b-form-input id="item"
                    type="text"
                    v-model="form.itemName"
                    :state="!$v.form.itemName.$invalid" 
                    aria-describedby="itemLiveFeedback">
                </b-form-input>
                <b-form-invalid-feedback id="imageLiveFeedback">
                    Please enter a valid item
                </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="descriptionInputGroup" label="Item Description" label-for="description">
                <b-form-input id="description"
                    type="text"
                    v-model="form.itemDescription"
                    :state="!$v.form.itemDescription.$invalid" 
                    aria-describedby="itemDescriptionLiveFeedback">
                </b-form-input>
                <b-form-invalid-feedback id="itemDescriptionLiveFeedback">
                    Please enter item description
                </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="priceInputGroup" label="Price" label-for="price">
                <b-form-input id="price"
                    type="number"
                    v-model="form.itemPrice"
                    :state="!$v.form.itemPrice.$invalid" 
                    aria-describedby="itemPriceDescriptionLiveFeedback">
                </b-form-input>
                  <b-form-invalid-feedback id="itemPriceLiveFeedback">
                    Please enter valid price
                </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group id="numberAvailableInputGroup" label="Number Available" label-for="numberAvailable">
                <b-form-input id="numberAvailable"
                    type="number"
                    v-model="form.numberAvailable"      
                    :state="!$v.form.numberAvailable.$invalid" 
                    aria-describedby="numberAvailableLiveFeedback">
                </b-form-input>
                  <b-form-invalid-feedback id="numberAvailableLiveFeedback">
                    Please enter number Available
                </b-form-invalid-feedback>
            </b-form-group>

              <b-form-group id="sizeInputGroup" label-for="size">
                <b-form-input id="size"
                    type="text"
                    v-model="form.itemSize"
                    placeholder="Enter Size... ex: sm, md or lg">
                </b-form-input>
            </b-form-group>

            <b-button type="submit" @click.prevent="createItem" :disabled="$v.form.$invalid" >Add Item</b-button>

        </b-form>
    </b-container>
</template>

<script>

    import { validationMixin } from "vuelidate";
    import { required, minLength } from "vuelidate/lib/validators";
        export default {
            data(){
                return{
                    form:{
                        itemName:'',
                        itemDescription:'',
                        itemImage:null,
                        itemPrice:'',
                        numberAvailable:'',
                        itemSize:'',
                        url:null,
                        sent:false
                    },
                }
            },
            mixins: [
                validationMixin
            ],
                validations: {
            form: {
                itemImage: {
                    required
                },
                itemName: {
                    required,
                    minLength: minLength(3),
                },
                itemDescription: {
                    required,
                    minLength: minLength(3)
                },
                itemPrice:{
                    required,
                    minLength: minLength(3),
                },
                numberAvailable:{
                    required
                }
            }
        },

        methods:{
            createItem(){

                console.log("INSIDE CREATE ITEM");
                // this.$v.form.$touch();

                // if (this.$v.form.$invalid) {
                    let formData = new FormData();

                    Object.keys(this.form).forEach(key => {
                        formData.append(key, this.form[key])
                    })
                    
                    axios.post("/api/items", formData, {headers: {'Content-Type': 'multipart/form-data'}}).then(({data}) => {

                        this.$notify({
                        group: 'notifications',
                        title: 'Success',
                        type: 'success',
                        text: 'Item Successfully added',
                        duration: '6000',
                        width: '100%'
                    });

                        // this.$store.dispatch('formSuccess')
                        this.resetForm()

                    }).catch((error) => {

                        console.log("ERROR: " + error);
                        // this.$store.dispatch('formError')
                    })
                // }
            },
            resetForm() {
                /* Reset our form values */
                this.form.itemName = ''
                this.form.itemDescription = ''
                this.form.itemSize = ''
                this.form.itemPrice = ''
                this.form.numberAvailable = ''
                this.form.itemImage = null
                this.form.url = null
                

                /* reset/clear native browser form validation state */
                this.show = false
                this.$nextTick(() => {
                    this.show = true;
                    this.$router.push('/');
                })
            },
            onImageChange(e){
                const file = e.target.files[0];
                this.form.url = URL.createObjectURL(file);
                this.form.image = file;            
            },
        },
    }
</script>

<style>
    .addItem{
        padding-top: 2rem ;
    }
</style>
