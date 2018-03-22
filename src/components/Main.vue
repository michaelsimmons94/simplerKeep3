<template>
  <v-app>
      <v-container grid-list-xl>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card>

            <div class="title">                      
            <v-text-field 
            placeholder= "Title..." 
            type="title" 
            v-model="current_title"
            full-width>
            </v-text-field>
            </div>
            <v-text-field
            v-model="current_text" 
            name="poem-input"
            placeholder="Write here"
            single-line
            multi-line
            auto-grow
            full-width
            value="current_text"
            class="input">
            
            </v-text-field>
            <span><v-btn @click.prevent="saveChanges(temp_item)" flat >Done</v-btn>
              <v-btn color="primary" flat @click.prevent="closeEdit()">Close</v-btn>
              </span>
            
          </v-card>
        </v-dialog>
        
          <v-layout row wrap align-center>
                <v-flex xs6 offset-xs3>
                     <v-card b80 >
                        <v-form @submit.prevent >  
                            <div class="title">                      
                            <v-text-field 
                            placeholder= "Title..." 
                            type="title" 
                            v-model="title"
                            full-width>
                            </v-text-field>
                            </div>
                            <v-text-field v-model="text"
                            name="poem-input"
                            placeholder="Write here"
                            single-line
                            multi-line
                            auto-grow
                            full-width
                            class="input">
                            </v-text-field>
                            <span><v-btn @click.prevent="addItem"  flat >Done</v-btn></span>
                        </v-form>
                        
                    </v-card>
                </v-flex>
                <v-flex xs6 offset-xs3 v-for="item in items" :key=item>
                    <v-card draggable="true" v-on:dragstart="dragItem(item)" 
                    v-on:dragover.prevent v-on:drop="dropItem(item)" 
                    @dblclick="edit(item)">
                            <v-card-title>
                                <div class="title">{{item.title}}</div>
                            </v-card-title>
                            <v-card-text>  
                                {{item.text}}
                            </v-card-text>
                            <v-card-actions>
                                <v-btn small flat v-on:click="edit(item)">
                                    Edit
                                </v-btn>
                                <v-btn small flat v-on:click="deleteItem(item)" >
                                    Delete
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                </v-flex>
            </v-layout>
      </v-container>
      <v-footer>
          <a href="https://github.com/michaelsimmons94/simplerKeep3.git">Get the orignial code here!</a>
      </v-footer>
  </v-app>
</template>
<script>
 import axios from 'axios';
 export default {
   name: 'Main',
   data () {
     return {
       items: [],
       title: '',
       text: '',
       show: 'all',
       drag: {},
       dialog: false,
       current_text:'',
       current_title:'',
       item_id: 0,
       temp_item:{}
       
     }
   },
   created: function() {
     this.getItems();
   },
   methods: {
     getItems: function() {
       axios.get("/api/items").then(response => {
	 this.items = response.data;
	 return true;
       }).catch(err => {
       });
     },
     addItem: function() {
       axios.post("/api/items", {
     title: this.title,
     text: this.text,
     edit: false,
       }).then(response => {
     this.title = "";
     this.text="";
	 this.getItems();
	 return true;
       }).catch(err => {
       });
     },
     deleteItem: function(item) {
       axios.delete("/api/items/" + item.id).then(response => {
	 this.getItems();
	 return true;
       }).catch(err => {
       });
     },
     
     dragItem: function(item) {
       this.drag = item;
     },
     dropItem: function(item) {
       axios.put("/api/items/" + this.drag.id, {
     title: this.drag.title,
     text: this.drag.text,
	 orderChange: true,
	 orderTarget: item.id
       }).then(response => {
	 this.getItems();
	 return true;
       }).catch(err => {
       });
     },
     edit: function(item){
         
         this.current_text= item.text;
         this.current_title=item.title;
         this.dialog=true;
         this.temp_item=item;
     },
     
     saveChanges: function(item){
          axios.put("/api/items/" + item.id, {
             title: this.current_title,
             text: this.current_text,
         }).then(response=>{
             this.getItems();
             this.closeEdit();
             return true;
         }).catch(err =>{console.log("save changes error")});
     },
     
       closeEdit:function(){
       this.current_text="";
       this.current_title="";
       this.dialog=false;
   }
   },

 }
</script>
<style scoped>
.title{
    font-size:3em;
    font-weight: bold;
}
.headline{
    font-size: 1.5em;
}
 ul {
     list-style: none;
 }

 li {
     width: auto;
     min-height: 40px;
     padding-left: 10px;
     margin-bottom: 3px;
     font-size: 1em;
     display: flex;
     align-items: center;
     
 }

 .delete {
     display: none;
     margin-left: auto;
 }

 li:hover .delete {
     display: block;
 }

 label {
     width: 400px;
 }
</style>