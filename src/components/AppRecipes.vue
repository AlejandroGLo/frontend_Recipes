<template>
  <div class="jumbotron vertical-center">
      <div class = "container">
          <div class = "row">
              <div class = "col-sm-12">
                  <h1>Recipes</h1>
                  <hr />
                  <!-- Allert Message -->
        <b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

        <button
          type="button"
          class="btn btn-success btn-sm"
          v-b-modal.recipe-modal
        >
          Create Recipe
        </button>
                  <br /> <br />
                  <table class = "table table-hover">
                      <thead>
                          <tr>
                              <th scope = "col"> Name </th>
                              <th scope = "col"> Ingredients </th>
                              <th scope = "col"> Instructions </th>
                              <th scope = "col"> Favorite </th>
                              <th scope = "col"> Rating </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for= "recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingredients }} </td>
                              <td> {{ recipe.instructions }} </td>
                              <td>
                                  <b-icon style="margin-left:20px"
                                  v-if="recipe.favorite == true" icon="check-circle-fill" variant="info" >
                                  </b-icon>
                                  <b-icon v-if="(recipe.favorite == false)" >
                                  </b-icon>
                              </td>
                                <td> 
                                    <div>
                            
                                       
                                         <b-form-rating id="rating-sm-no-border" v-model="recipe.rating" no-border size="sm"></b-form-rating>
                                        
                                        <p class="mt-2"></p>
                                    </div> 
                                 </td>
                              
                               <td> 
                                  <div class="btn-group" role="group">
                                      <button
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      v-b-modal.edit-recipe-modal
                                      @click="editRecipe(recipe)"
                                      >
                                      Edit
                                      </button>
                                      <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="deleteRecipe(recipe)"
                                      >
                                      Delete
                                      </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <footer class = "text-center">
                      <p> &copy; 2022- 2023 </p>
                  </footer>
              
              </div>
          </div>


          <!-- Beginning of Modal for Create Recipe-->
              <div>   
            <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="Add Recipe"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group
                  id="form-name-group"
              label="Name:"
              label-for="form-name-input"
              description="Enter the name of the recipe."
              >
              <b-form-input
              id="form-name-input"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="Recipe Name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group
              id="form-ingredients-group"
              label="Ingredients:"
              label-for="form-ingredients-input"
              description="Enter the ingredients for the recipe separated by commas."
              >
              <b-form-input
              id="form-ingredients-input"
              type="text"
              v-model="createRecipeForm.ingredients"
              placeholder="Recipe Ingredients"
              required
              ></b-form-input>
              </b-form-group>

              <b-form-group
              id="form-instructions-group"
              label="Instructions:"
              label-for="form-instructions-input"
              description="Instructions: "
              >
              <b-form-input
              id="form-instructions-input"
              type="text"
              v-model="createRecipeForm.instructions"
              placeholder="What are the steps to make this recipe?"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-favorite-group"    
              label="Favorite:"
              label-for="form-favorite-input"
              description="Is this recipe your favorite?"
              >
              <b-form-checkbox
              id="form-favorite-input"
              v-model="createRecipeForm.favorite"
              switch
              ></b-form-checkbox>
              </b-form-group>
              <b-form-group
              id="form-rating-group"
              label="Rating:"
              label-for="form-rating-input"
              description="Give this recipe a rating from 1 to 5 (5 being the best)"
              >
              <b-form-rating
              id="form-rating-input"
              type="integer"
              v-model="createRecipeForm.rating"
              ></b-form-rating>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>

          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
     
                
          <b-modal
            ref = "editRecipeModal"
            id="edit-recipe-modal"
            title="Edit Recipe"
            hide-backdrop
            hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-edit-name-group"
          label="Name:"
          label-for="form-edit-name-input"
          description="Enter the name of the recipe."
          >
          <b-form-input
          id="form-edit-name-input"
          type="text"
          v-model="updateRecipeForm.name"
            >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-edit-ingredients-group"
          label="Ingredients:"
          label-for="form-edit-ingredients-input"
          description="Edit the ingredients of the recipe."
          >
          <b-form-input
          id="form-edit-ingredients-input"
          type="text"
          v-model="updateRecipeForm.ingredients"
          
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-edit-instructions-group"
          label="Instructions:"
          label-for="form-edit-instructions-input"
          description="Edit the steps to make this recipe."
          >
          <b-form-input

          id="form-edit-instructions-input"
          type="text"
          v-model="updateRecipeForm.instructions"
          
          >
          </b-form-input>
          </b-form-group>

          <b-form-group

          id="form-edit-favorite-group"
          label="Favorite:"
          label-for="form-edit-favorite-input"
          description="Edit the favorite status of this recipe."
          >
          <b-form-checkbox
          id="form-favorite-input"
          v-model="updateRecipeForm.favorite"
          switch
          ></b-form-checkbox>
          </b-form-group>
          <b-form-group
          id="form-edit-rating-group"
          label="Rating:"
          label-for="form-edit-rating-input"
          description="Edit the rating of this recipe."
          >
          <b-form-rating
          id="form-edit-rating-input"
          type="integer"
          v-model="updateRecipeForm.rating"
          
          ></b-form-rating>
          </b-form-group>
          <b-button type="submit" variant="outline-info">Update</b-button>
          </b-form>
          </b-modal>
          <!-- End of Modal for Edit Recipe-->


      </div>
  
  </div>
</div>
 
  

</template>

<script>


import axios from 'axios';
export default {
  name: 'AppRecipes',
  data(){
      return {
          recipes: [],
          createRecipeForm: {
                id: '',
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                rating: 0,
          },

          updateRecipeForm: {
                id: '',
                name: '',
                ingredients: '',
                instructions: '',
                favorite: false,
                rating: 0,
          },

          showMessage: false,
          message: '',

      };
  },
  methods: {

    gotohome(){
            this.$router.push('/')
        },

    /***************************************************
     * RESTful requests
     ***************************************************/
    
      //GET Recipes

      RESTgetRecipes() {
      const path = `${process.env.VUE_APP_ROOT_URL}/`;
      axios
          .get(path)
          .then((response) => {
              this.recipes = response.data.recipes;
          })
          .catch((error) => {
              console.log(error);
          });
      },

      //POST Recipes
      RESTcreateRecipe(payload){
          const path = `${process.env.VUE_APP_ROOT_URL}/`;
          axios
              .post(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Recipe Created Successfully!";
                  // To actually show the message
                  this.showMessage = true;
                  // To hide the message after 3 seconds
                  setTimeout(() => {
                    this.showMessage = false;
                  }, 3000);
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },

      RESTupdateRecipe(payload, recipeId){
          const path = `${process.env.VUE_APP_ROOT_URL}/${recipeId}`;
          axios
              .put(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
                    this.message = "Recipe Edited Successfully!"
                    this.showMessage = true;
                    setTimeout(() => {
                        this.showMessage = false;
                    }, 3000);
                    
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },


      RESTdeleteRecipe(id){
          const path = `${process.env.VUE_APP_ROOT_URL}/${id}`;
          axios
              .delete(path)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Recipe Deleted Successfully!"
                  this.showMessage = true;
                  set.TimeOut(() => {
                      this.showMessage = false;
                  }, 3000);

              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },

      initForm(){
          this.createRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };
          this.updateRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };

      },
      onSubmit(evt) {
          evt.preventDefault(); 
          this.$refs.addRecipeModal.hide(); 
          const payload = {
              name: this.createRecipeForm.name,
              ingredients: this.createRecipeForm.ingredients,
              instructions: this.createRecipeForm.instructions,
              favorite: this.createRecipeForm.favorite,
              rating: this.createRecipeForm.rating
          };
          this.RESTcreateRecipe(payload);
          this.initForm();
      },
      onSubmitUpdate(evt) {
          evt.preventDefault(); 
          this.$refs.editRecipeModal.hide(); 
          const payload = {
              name: this.updateRecipeForm.name,
              ingredients: this.updateRecipeForm.ingredients,
              instructions: this.updateRecipeForm.instructions,
              favorite: this.updateRecipeForm.favorite,
              rating: this.updateRecipeForm.rating
          };
          this.RESTupdateRecipe(payload, this.updateRecipeForm.id);
          this.initForm();
      },

      deleteRecipe(recipe) {
          this.RESTdeleteRecipe(recipe.id);
      },

      editRecipe(recipe) {
          this.updateRecipeForm = recipe;
      },

      

  },

   /***************************************************
 * LIFECYClE HOOKS
 ***************************************************/

  created() {
      this.RESTgetRecipes();
  },
};

</script>
