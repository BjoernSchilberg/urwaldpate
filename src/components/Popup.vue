<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="485">
      <v-card>
        <v-card-title class="headline">
          Ja, ich werde Urwald-Pate für {{ hektar }} Hektar!
        </v-card-title>

        <v-card-text>
          Ich schütze mit
          <strong>{{ betrag }} Euro monatlich</strong> Urwald von morgen im {{ gebiet }}.
        </v-card-text>

        <v-card-actions class="justify-center">
          <v-btn
            small
            outlined
            color="rgb(118,184,40)"
            @click="more">
            Weitere Fläche wählen!
          </v-btn>
          <v-btn
            small
            color="rgb(118,184,40)"
            dark
            @click="klick">
            Jetzt Urwald-Pate werden!
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<style>
.headline {
  word-break: normal !important;
}
</style>

<script>
export default {
  props: { dialog: Boolean, selectedFeatures: {
    type: Array,
    // Old style: function () { return [] }. ES6:
    default: ()=>[]
    },
    gebiet: {type: String, required: true, default: ""}
    },
  data: ()=>({
  }),
  computed: {
    selectedFeaturesLocal: {
      get: function(){
        return this.selectedFeatures
      },
      set: function(value){
        this.$emit("update:selectedFeatures",value)
      }
    },
    betrag: function() {
      return this.hektarIDs.length * 10;

    },
    hektar: function() {
      return this.hektarIDs.length * 1;
    },
    hektarIDs: function() {
      var hektarIDs = [];
      if (this.selectedFeaturesLocal.length > 0) {
        this.selectedFeaturesLocal.forEach(element => {
          hektarIDs.push(element.properties.RasterID);
        });
        return hektarIDs;
      } else {
        return [];
      }
    }
  },
  methods: {
    more() {
      this.$emit("update:dialog", !this.dialog);
    },
    klick() {
      window.open(
        "https://naturerbe.nabu.de/spenden-und-helfen/patenschaften/include/formular/urwald.html?hektar-id="+
          this.hektarIDs +
          "&betrag=" +
          this.betrag
      );
      this.$emit("update:dialog", !this.dialog);
      this.selectedFeaturesLocal=[];
    }
  }
};
</script>
