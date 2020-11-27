<template>
  <div>
    <a-row
      type="flex"
      justify="space-around"
      align="middle"
      style="margin-top: 30px;"
    >
      <a-col :span="20">
        <v-layout align-start>
          <v-flex>
            <v-data-table
              :headers="headers"
              :items="persons"
              :search="search"
              class="elevation-1"
            >
              <template v-slot:top>
                <v-toolbar flat color="white">
                  <v-toolbar-title>Usuário</v-toolbar-title>

                  <v-spacer></v-spacer>

                  <v-text-field
                    class="text-xs-center"
                    v-model="search"
                    label="Pesquisa"
                    single-line
                    hide-details
                  ></v-text-field>

                  <v-spacer></v-spacer>
                </v-toolbar>
              </template>

              <template v-slot:[`item.action`]="{ item }">
                <a-tooltip placement="right">
                  <template slot="title">
                    <span>Editar</span>
                  </template>

                  <v-icon @click="editar(item)">
                    mdi-table-edit
                  </v-icon>
                </a-tooltip>
              </template>
            </v-data-table>
          </v-flex>

          <a-modal
            title="Editar Usuário"
            :visible="visible"
            centered
            @cancel="cancelarEdicao"
          >
            <a-form>
              <a-row :gutter="24">
                <a-col :span="24">
                  <a-form-item label="Nome">
                    <a-input v-model="editUsuario.nome"></a-input>
                  </a-form-item>
                </a-col>

                <a-col :span="24">
                  <a-form-item label="Usuário">
                    <a-input v-model="editUsuario.usuario"></a-input>
                  </a-form-item>
                </a-col>
              </a-row>
            </a-form>

            <template slot="footer">
              <a-button key="back" @click="cancelarEdicao">
                Cancelar
              </a-button>

              <a-button key="submit" type="primary" @click="alterar">
                Alterar
              </a-button>
            </template>
          </a-modal>
        </v-layout>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      configuration: {
        headers: {
          Token: this.$store.state.portarias_token,
        },
      },
      usuario: this.$store.state.usuario,
      dialog: false,
      search: "",
      persons: [],
      headers: [
        { text: "Opções", value: "action", sortable: false },
        { text: "Nome", value: "nome", sortable: true },
        { text: "Login", value: "usuario", sortable: false },
      ],
      editedIndex: -1,
      editUsuario: {
        id: "",
        nome: "",
        usuario: "",
      },

      adModal: false,
      adAccion: 0,
      adNome: "",
      adId: "",
      visible: false,
    };
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
  },

  created() {
    this.listar();
  },

  methods: {
    editar(item) {
      this.visible = true;
      this.editUsuario.nome = item.nome;
      this.editUsuario.usuario = item.usuario;
      this.editUsuario.id = item.id;
    },

    alterar() {
      let me = this;
      axios
        .put(
          `/usuarios/${this.editUsuario.id}`,
          this.editUsuario,
          this.configuration
        )
        .then((res) => {
          if (res.data.success) {
            this.$message.success("Usuário Atualizado!");
          }
          me.limparEdicao();
          me.listar();
        })
        .catch(function(error) {
          // eslint-disable-line no-unused-vars
        });
    },

    cancelarEdicao() {
      this.visible = false;
    },

    limparEdicao() {
      this.visible = false;
      this.formNome = "";
      this.formUsuario = "";
      this.formId = "";
    },

    listar() {
      const me = this;
      let rota = "/usuarios";
      // let rota = `/usuarios/${this.usuario.id}`;

      axios
        .get(rota, this.configuration)
        .then(function(response) {
          me.persons = response.data.usuarios;
        })
        .catch(function(error) {
          // eslint-disable-line no-unused-vars
        });
    },

    close() {
      this.limpar();
      this.dialog = false;
    },

    limpar() {
      this.adId = "";
    },
  },
};
</script>
