<template>
  <div id="teste">
    <v-container>
      <v-data-table
        :search="search"
        :headers="headers"
        :items="lancamentosUsuario"
        class="elevation-1"
<<<<<<< HEAD
=======
        :footer-props="{
          itemsPerPageOptions: [-1, 10, 20, 30],
          'items-per-page-text': 'Lançamentos por página: ',
        }"
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
      >
        <!-- Campo Pesquisa e Filtragem de processos -->
        <template v-slot:[`item.status`]="{ item }">
          <span v-if="item.status == 'Confirmado'">
            <v-icon style="color: #49D907; ">
              mdi-checkbox-marked-circle-outline
            </v-icon>
          </span>
          <span v-if="item.status == 'Pendente'">
            <v-icon style="color: gray; ">
              mdi-checkbox-blank-circle-outline
            </v-icon>
          </span>
          <span v-if="item.status == 'Negativo'">
            <v-icon style="color: #F24607;">
              mdi-close-circle-outline
            </v-icon>
          </span>
        </template>

        <template v-slot:[`item.action`]="{ item }">
          <a-tooltip placement="left">
            <template slot="title">
              <span>Excluir</span>
            </template>

            <v-icon
              big
              class="mr-2"
              @click="excluir(item)"
              style="color: #F24607;"
            >
              mdi-close-thick
            </v-icon>
          </a-tooltip>

          <a-tooltip placement="left">
            <template slot="title">
              <span>Editar</span>
            </template>

            <v-icon big class="mr-2" @click="editar(item)">
              mdi-table-edit
            </v-icon>
          </a-tooltip>
        </template>

        <template v-slot:[`item.tipo_lancamento`]="{ item }">
          <samp
            style="color: #49D907; font-weight: bold;"
            v-if="item.tipo_lancamento == 'Receita'"
            >Receita</samp
          >
          <samp
            style="color: #F24607; font-weight: bold;"
            v-if="item.tipo_lancamento == 'Despesa'"
            >Despesa</samp
          >
        </template>
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Lançamentos</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-text-field
              class="text-xs-center"
              v-model="search"
              label="Pesquisa"
              single-line
              hide-details
            ></v-text-field>
<<<<<<< HEAD
=======
            <v-spacer></v-spacer>

            <v-select
              v-model="value"
              :items="items"
              label="Filtrar lançamento por"
              single-line
              hide-details
            ></v-select>
            <v-spacer></v-spacer>
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f

            <v-spacer></v-spacer>
            <a-button type="primary" @click="showModal">
              Adicionar Lançamento <a-icon type="plus" />
            </a-button>
<<<<<<< HEAD
=======
            <!-- <v-spacer></v-spacer>
            <a-button type="primary" @click="gerarRelatorio()">
              Relatório Anual<a-icon type="printer" />
            </a-button> -->
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
          </v-toolbar>
        </template>
      </v-data-table>
    </v-container>

    <a-modal
      v-model="visibleAdd"
      :visible="visibleAdd"
      @ok="adicionarLancamento"
      okText="Adicionar"
      cancelText="Cancelar"
      @cancel="cancel()"
      centered
    >
      <!--   -->
      <a-form :form="form" style="padding: 10px;" hideRequiredMark>
        <a-form-item label="Descrição">
          <a-input
            placeholder="Descrição"
            v-model="descricao"
            v-decorator="[
              'descricao',
              {
                rules: [{ required: true, message: 'Informe a descrição.' }],
              },
            ]"
          />
        </a-form-item>
        <a-form-item label="Valor">
          <a-input-number
<<<<<<< HEAD
            placeholder="Valor"
            v-model="valor"
            :min="1"
            :formatter="
              (value) => `$ ${value}`.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
            "
=======
            placeholder="Valor R$"
            v-model="valor"
            :min="1"
            :formatter="
              (value) => `${value}`.replace(/\B(?=(\d{3})+(?!\d))/g, ',')"
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
            :parser="(value) => value.replace(/\$\s?|(,*)/g, '')"
            v-decorator="[
              'valor',
              {
                rules: [{ required: true, message: 'Informe o valor.' }],
              },
            ]"
          />
        </a-form-item>

        <a-form-item label="Data">
          <a-date-picker
            v-model="data"
            :locale="locale"
            :format="dateFormat"
            v-decorator="[
              'data',
              { rules: [{ required: true, message: 'Data necessaria' }] },
            ]"
          />
        </a-form-item>
        <a-form-item label="Tipo Lançamento">
          <!-- <a-input v-model="editSetor.status_bot"></a-input> -->
          <a-select v-model="tipo_l">
            <a-select-option
              v-for="tipo in tipo_lancamento"
              :key="tipo"
              :value="tipo"
            >
              {{ tipo }}
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item label="Status Lançamento">
          <!-- <a-input v-model="editSetor.status_bot"></a-input> -->
          <a-select v-model="status_lancamento">
            <a-select-option v-for="tipo in status" :key="tipo" :value="tipo">
              {{ tipo }}
            </a-select-option>
          </a-select>
        </a-form-item>
      </a-form>
    </a-modal>

    <!-- EDITAR INFORMAÇÕES -->
    <a-modal
      title="Editar lancamentos"
      :visible="visible"
      centered
      @cancel="cancelarEdicao"
    >
      <a-form>
        <a-row :gutter="24">
          <a-col :span="24">
            <a-form-item label="Descrição">
              <a-input v-model="editSetor.descricao"></a-input>
            </a-form-item>
          </a-col>
          <a-col :span="24">
            <a-form-item label="Valor">
              <a-input v-model="editSetor.valor"></a-input>
            </a-form-item>
          </a-col>
          <!-- <a-col :span="24">
            <a-form-item label="Data">
              <a-date-picker
                v-model="editSetor.data"
                :locale="locale"
                :format="dateFormat"
                v-decorator="[
                  'data',
                  { rules: [{ required: true, message: 'Data necessaria' }] },
                ]"
              />
            </a-form-item>
          </a-col> -->
          <a-col :span="24">
            <a-form-item label="Status">
              <!-- <a-input v-model="editSetor.status_bot"></a-input> -->
              <a-select v-model="editSetor.status">
                <a-select-option
                  v-for="tipo in status"
                  :key="tipo"
                  :value="tipo"
                >
                  {{ tipo }}
                </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
        </a-row>
      </a-form>

      <template slot="footer">
        <a-button key="back" @click="cancelarEdicao">Cancelar</a-button>
        <a-button key="submit" type="primary" @click="alterar"
          >Alterar</a-button
        >
      </template>
    </a-modal>
  </div>
</template>

<script>
import locale from "ant-design-vue/es/date-picker/locale/pt_BR";
import axios from "axios";
<<<<<<< HEAD
=======
// import 'moment/locale/pt-BR';
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f

export default {
  data() {
    return {
      configuration: {
        headers: {
          Token: this.$store.state.portarias_token,
        },
      },
      check: null,
      host: this.axios.defaults.baseURL,
      usuario: this.$store.state.usuario,
      locale,
      dateFormat: "DD/MM/YYYY",
      search: "",
      lancamentos: [],
<<<<<<< HEAD
      lancamentosUsuario:[],
=======
      lancamentosUsuario: [],
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
      expanded: [],
      arquivo_upload: "",
      headers: [
        { text: "Ações", value: "action", sortable: false },
        { text: "Descricao ", value: "descricao" },
        { text: "Valor", value: "valor" },
        { text: "Data", value: "data" },
        { text: "Status", value: "status" },
        { text: "Tipo", value: "tipo_lancamento" },
      ],

      uploadVisible: false,
      idUpload: "",
      file: null,

      // Anexos
      anexoVisible: false,
      anexos: [],

      visibleAdd: false,
      form: this.$form.createForm(this),
      numero_processo: "",
      destinatario: "",
      data: "",
      // NOVO
      descricao: "",
      valor: "",

<<<<<<< HEAD

=======
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
      tipo_l: "",

      visible: false,

      tipo_lancamento: ["Receita", "Despesa"],
      status: ["Pendente", "Confirmado", "Negativo"],
<<<<<<< HEAD
      status_lancamento: "Lançado",
=======
      status_lancamento: "",
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
      editSetor: {
        id: "",
        descricao: "",
        valor: "",
        // data: "",
        status: "",
      },
      idExcluir: "",
<<<<<<< HEAD
=======

      items: [
        "Todos",
        "Confirmados",
        "Pendentes",
        "Negativados",
        "Receitas",
        "Despesas",
      ],
      value: "",
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
    };
  },

  created() {
    this.listaLancamentos();
<<<<<<< HEAD
    this.listaLancamentosUsuario()
=======
    this.listaLancamentosUsuario();
  },

  watch: {
    dialog(val) {
      val || this.close();
    },

    /* eslint-disable no-unused-vars */
    value(newVal, oldVal) {
      switch (newVal) {
        case "Todos":
          this.expanded = [];
          this.listaLancamentosUsuario();
          break;
        case "Confirmados":
          this.expanded = [];
          this.listaLancamentosConfirmados();
          break;
        case "Pendentes":
          this.expanded = [];
          this.listaLancamentosPendentes();
          break;
        case "Negativados":
          this.expanded = [];
          this.listaLancamentosNegativados();
          break;
        case "Receitas":
          this.expanded = [];
          this.listaLancamentosReceitas();
          break;
        case "Despesas":
          this.expanded = [];
          this.listaLancamentosDespesas();
          break;
      }
    },
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
  },

  methods: {
    listaLancamentos() {
      let rota = "/lancamentos";
<<<<<<< HEAD
      
=======

>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
      this.axios
        .get(rota, this.configuration)
        .then((res) => {
          this.lancamentos = res.data.lancamentos;
          console.log(res.data);
          for (let pro of this.lancamentos) {
            pro.data = new Date(pro.data).toLocaleString();
            pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
          }
        })
        .catch((e) => {
          // eslint-disable-line no-unused-vars
        });
    },

<<<<<<< HEAD
    listaLancamentosUsuario(){
      let rota = `lancamentos/usuario/${this.usuario.id}`
      
      this.axios.get(rota, this.configuration).then((res) =>{
          this.lancamentosUsuario = res.data.lancamentos          
          for (let pro of this.lancamentosUsuario) {
            pro.data = new Date(pro.data).toLocaleString();
            pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
          }

      })

=======
    listaLancamentosUsuario() {
      let rota = `lancamentos/usuario/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
    },

    listaLancamentosConfirmados() {
      let rota = `lancamentos/confirmados/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
    },

    listaLancamentosPendentes() {
      let rota = `lancamentos/pendentes/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
    },

    listaLancamentosNegativados() {
      let rota = `lancamentos/negativo/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
    },

    listaLancamentosReceitas() {
      let rota = `lancamentos/receita/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
    },
    listaLancamentosDespesas() {
      let rota = `lancamentos/despesa/${this.usuario.id}`;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          pro.data = new Date(pro.data).toLocaleString();
          pro.valor = pro.valor.replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
      });
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
    },

    adicionarLancamento(e) {
      const me = this;
      let data = new FormData();

      this.form.validateFields((err, values) => {
        if (!err) {
          this.axios
            .post(
              "/lancamentos/",
              {
                descricao: this.descricao,
                valor: this.valor,
                status: this.status_lancamento,
                data: this.data,
                tipo_lancamento: this.tipo_l,
                status_lancamento: this.status_lancamento,
                usuario_id: this.usuario.id,
              },
              /* data,*/ this.configuration
            )
            .then((res) => {
              this.visibleAdd = false;
              this.listaLancamentos();
<<<<<<< HEAD
              this.listaLancamentosUsuario()
=======
              this.listaLancamentosUsuario();
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
              this.cancel();
            });
        }
      });
    },

    showAnexoModal(anexos) {
      this.anexoVisible = true;
      this.anexos = anexos;
    },

    showModal() {
      this.visibleAdd = true;
    },

    cancel() {
      this.form.resetFields();
    },

    cancelAnexoModal() {
      this.anexoVisible = false;
      this.anexos = [];
    },

    setFile(e) {
      var files = e.target.files;
      this.file = files[0];
    },

    editar(item) {
      this.visible = true;
      this.editSetor.descricao = item.descricao;
      this.editSetor.valor = item.valor;
      this.editSetor.status = item.status;
      this.editSetor.id = item.id;
      // this.editSetor.data = item.data;
<<<<<<< HEAD
      console.log(this.editSetor.status)
=======
      console.log(this.editSetor.status);
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
    },

    cancelarEdicao() {
      this.visible = false;
    },

    alterar() {
      let me = this;
      me.axios
        .put(`/lancamentos/${me.editSetor.id}`, me.editSetor, me.configuration)
        .then((res) => {
          if (res.data.success) {
            this.$message.success("Lançamento Atualizado!");
          }
          me.limparEdicao();
          me.listaLancamentos();
<<<<<<< HEAD
          me.listaLancamentosUsuario()
=======
          me.listaLancamentosUsuario();
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
        })
        .catch(function(error) {
          // eslint-disable-line no-unused-vars
        });
    },
    limparEdicao() {
      this.visible = false;
      this.formNome = "";
      this.formUsuario = "";
      this.formId = "";
      this.check = "";
      this.arquivo_upload = "";
    },
    handleReset() {
      this.form.resetFields();
    },

    excluir(item) {
      const me = this;

      this.$confirm({
        title: "Deseja excluir esse item?",
        onOk() {
          me.axios
            .delete(`lancamentos/delete/${item.id}`, me.configuration)
            .then((response) => {
              if (response.data.success) {
                this.$message.success("Lancamentos Removido!");
              }
              me.listaLancamentos();
<<<<<<< HEAD
              me.listaLancamentosUsuario()
=======
              me.listaLancamentosUsuario();
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
            });
        },
      });
    },
<<<<<<< HEAD
=======

>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
  },
};
</script>

<style>
#header {
  display: block;
  background-color: #ffffff;
  max-width: 768px;
  margin: 0 auto;
  text-align: center;
  font-weight: bold;
  font-size: 25px;
  color: #2a3d29;
}

#header img {
  margin: 30px;
}

.home {
  padding: 0 10px;
}

.ant-table-thead > tr > th {
  background: #7fa07d !important;
  font-weight: bold !important;
}

.ant-table-thead
  > tr.ant-table-row-hover:not(.ant-table-expanded-row):not(.ant-table-row-selected)
  > td,
.ant-table-tbody
  > tr.ant-table-row-hover:not(.ant-table-expanded-row):not(.ant-table-row-selected)
  > td,
.ant-table-thead
  > tr:hover:not(.ant-table-expanded-row):not(.ant-table-row-selected)
  > td,
.ant-table-tbody
  > tr:hover:not(.ant-table-expanded-row):not(.ant-table-row-selected)
  > td {
  background: #eaf9ec !important;
}

.ant-table-title {
  background: #6b8669;
}

.ant-btn:hover,
.ant-btn:focus {
  color: #7fa07d !important;
  border-color: #7fa07d !important;
}

.ant-btn-primary {
  color: #fff !important;
  background-color: #7fa07d !important;
  border-color: #6b8669 !important;
}

.ant-btn-primary:hover,
.ant-btn-primary:focus {
  color: #fff !important;
  background-color: #a4c3a3 !important;
  border-color: #7fa07d !important;
}
</style>
