<template>
  <v-container class="grey lighten-5" v-if="show_results">
    <v-row no-gutters justify="center" align="center">
      <v-row>
        <v-col>Name: {{name}}</v-col>
        <v-col>Mnemonic: {{mnemonic}}</v-col>
        <v-col>Rank: {{rank}}</v-col>
        <v-col>Superregnum: {{superregnum}}</v-col>
      </v-row>

      <v-expansion-panels inset multiple v-model="panel">
        <v-expansion-panel>
          <v-expansion-panel-header>Linked Databases</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-card>
              <v-divider></v-divider>
              <v-chip-group active-class="deep-purple accent-6 white--text" column>
                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/nuccore/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Nucleotide</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/protein/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Protein</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/structure/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Structure</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/genome/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Genome</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/popset/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Popset</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/gds/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >GEO Datasets</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/pmc/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >PubMed Central</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/gene/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Gene</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/sra/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >SRA Experiments</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/ipg/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Identical Protein Groups</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/bioproject/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Bio Project</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/biosample/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Bio Sample</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/biosystems/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Bio Systems</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/assembly/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Assembly</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/probe/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Probe</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/pcassay/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >PubChem BioAssay</a>
                </v-chip>

                <v-chip>
                  <a
                    v-bind:href="'https://www.ncbi.nlm.nih.gov/taxonomy/?term=txid'+taxonomyId+'[Organism:exp]'"
                  >Taxonomy</a>
                </v-chip>
              </v-chip-group>
            </v-card>
          </v-expansion-panel-content>
        </v-expansion-panel>
        <v-expansion-panel>
          <v-expansion-panel-header>Siblings Links</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-progress-linear v-if="loaded_siblings" indeterminate color="cyan"></v-progress-linear>
            <v-data-iterator :items="siblingsLinks_items" :items-per-page.sync="itemsPerPage">
              <template v-slot:default="props">
                <v-row>
                  <v-col
                    v-for="item in props.items"
                    :key="item.name"
                    cols="12"
                    sm="6"
                    md="4"
                    lg="3"
                  >
                    <v-card>
                      <v-card-title class="subheading font-weight-bold">{{ item.scientificName }}</v-card-title>
                      <v-divider></v-divider>

                      <v-list dense>
                        <v-list-item>
                          <v-list-item-content>Mnemonic:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.mnemonic }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item v-if="item.commonName">
                          <v-list-item-content>CommonName:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.commonName }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item v-if="item.synonym">
                          <v-list-item-content>Synonym:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.synonym }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>Rank:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.rank }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>Superregnum:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.superregnum }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>ParentLink:</v-list-item-content>
                          <v-list-item-content class="align-end">
                            <a
                              v-bind:href="'https://www.uniprot.org/taxonomy/'+item.id"
                            >UniProt Link</a>
                            <a
                              v-bind:href="'https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Info&id='+item.id+'&lvl=3&lin=f&keep=1&srchmode=1&unlock'"
                            >NCBI Link</a>
                          </v-list-item-content>
                        </v-list-item>
                      </v-list>
                    </v-card>
                  </v-col>
                </v-row>
              </template>
            </v-data-iterator>
          </v-expansion-panel-content>
        </v-expansion-panel>
        <v-expansion-panel>
          <v-expansion-panel-header>Children Links</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-progress-linear v-if="loaded_chileds" indeterminate color="cyan"></v-progress-linear>
            <v-data-iterator :search="search" :items="items" :items-per-page.sync="itemsPerPage">
              <template v-slot:default="props">
                <v-row>
                  <v-col
                    v-for="item in props.items"
                    :key="item.name"
                    cols="12"
                    sm="6"
                    md="4"
                    lg="3"
                  >
                    <v-card>
                      <v-card-title class="subheading font-weight-bold">{{ item.scientificName }}</v-card-title>
                      <v-divider></v-divider>

                      <v-list dense>
                        <v-list-item>
                          <v-list-item-content>Mnemonic:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.mnemonic }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item v-if="item.commonName">
                          <v-list-item-content>CommonName:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.commonName }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item v-if="item.synonym">
                          <v-list-item-content>Synonym:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.synonym }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>Rank:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.rank }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>Superregnum:</v-list-item-content>
                          <v-list-item-content class="align-end">{{ item.superregnum }}</v-list-item-content>
                        </v-list-item>

                        <v-list-item>
                          <v-list-item-content>ParentLink:</v-list-item-content>
                          <v-list-item-content class="align-end">
                            <a
                              v-bind:href="'https://www.uniprot.org/taxonomy/'+item.id"
                            >UniProt Link</a>
                            <a
                              v-bind:href="'https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id='+item.id"
                            >NCBI Link</a>
                          </v-list-item-content>
                        </v-list-item>
                      </v-list>
                    </v-card>
                  </v-col>
                </v-row>
              </template>
            </v-data-iterator>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-row>
  </v-container>
</template>

<script>
import { bus } from "../main.js";
import axios from "axios";

export default {
  data: () => ({
    itemsPerPage: 8,
    panel: [0, 1, 2],
    taxonomyId: "",
    name: "",
    mnemonic: "",
    commonName: "",
    synonym: "",
    rank: "",
    superregnum: "",
    parentLink: "",
    childrenLinks: [],
    siblingsLinks: [],
    siblingsLinks_items: [],
    selectedLandscapes: [],
    items: [],
    search: "",
    show_results: false,
    loaded_siblings: false,
    loaded_chiled: false
  }),
  mounted() {},
  created() {
    bus.$on("search_reset", () => {
      this.show_results = false;
    });

    bus.$on("search_result", data => {
      this.result = data;
      this.show_results = true;
      this.taxonomyId = data.taxonomies[0].taxonomyId;
      this.name = data.taxonomies[0].scientificName;
      this.mnemonic = data.taxonomies[0].mnemonic;
      this.commonName = data.taxonomies[0].commonName;
      this.synonym = data.taxonomies[0].synonym;
      this.rank = data.taxonomies[0].rank;
      this.superregnum = data.taxonomies[0].superregnum;
      this.parentLink = data.taxonomies[0].parentLink;
      this.childrenLinks = data.taxonomies[0].childrenLinks;
      this.siblingsLinks = data.taxonomies[0].siblingsLinks;
      this.items = [];
      this.siblingsLinks_items = [];
      let i;
      if (this.childrenLinks != null) {
        this.loaded_chiled = true;
        for (i = 0; i < this.childrenLinks.length; i++) {
          axios
            .get(this.childrenLinks[i])
            .then(response => {
              let data = response.data;
              this.items.push({
                id: data.taxonomyId,
                mnemonic: data.mnemonic,
                scientificName: data.scientificName,
                commonName: data.commonName,
                synonym: data.synonym,
                rank: data.rank,
                superregnum: data.superregnum,
                parentLink: data.parentLink
              });
              this.loaded_chiled = false;
            })
            .catch(e => {
              this.errors.push(e);
            });
        }
      }
      if (this.siblingsLinks != null) {
        this.loaded_siblings = true;
        for (i = 0; i < this.siblingsLinks.length; i++) {
          axios
            .get(this.siblingsLinks[i])
            .then(response => {
              let data = response.data;
              this.siblingsLinks_items.push({
                id: data.taxonomyId,
                mnemonic: data.mnemonic,
                scientificName: data.scientificName,
                commonName: data.commonName,
                synonym: data.synonym,
                rank: data.rank,
                superregnum: data.superregnum,
                parentLink: data.parentLink
              });
              this.loaded_siblings = false;
            })
            .catch(e => {
              this.errors.push(e);
            });
        }
      }
    });
  }
};
</script>