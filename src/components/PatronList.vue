<template>
  <b-row>
    <b-col cols="12">
      <h2>
        Patron List   
      </h2>
      <b-btn href="#/add-patron">ADD Patron</b-btn>
      <b-table striped hover :items="patrons" :fields="fields">
        <template slot="actions" scope="row">
          <b-btn size="sm" @click.stop="details(row.item)">Details</b-btn>
        </template>
      </b-table>
    </b-col>
  </b-row>
</template>

<script>

import firebase from '../Firebase'
import router from '../router'

export default {
  name: 'PatronList',
  //returns the patron data from the database
  data () {
    return {
      //what data the table will populate with
      fields: {

        //patron name column
        name: { label: 'Name', sortable: true, 'class': 'text-left' },

        //patron action column
        actions: { label: 'Action', 'class': 'text-center' }
      },

      //array of patron data
      patrons: [],
      errors: [],
      //gets the patron table from the firebase database
      ref: firebase.firestore().collection('patron'),
    }
  },
  //pulls the data from the firebase database
  created () {
    this.ref.onSnapshot((querySnapshot) => {
      this.patrons = [];
      querySnapshot.forEach((doc) => {
        this.patrons.push({
          key: doc.id,
          name: doc.data().name
        });
      });
    });
  },

  //function that shows the patron finding the the specific id
  methods: {
    details (patron) {
      router.push({ name: 'ShowPatron', params: { id: patron.key }})
    }
  }
}
</script>

<style>
  .table {
    width: 96%;
    margin: 0 auto;
  }
</style>
