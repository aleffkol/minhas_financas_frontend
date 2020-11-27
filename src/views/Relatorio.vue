<template>
  <v-container>
    <div id="grafico1">
      <h1>Quantidade de Receita e Despesa</h1>
      <pie-chart
        :data="[
          ['Receita', qntReceita],
          ['Despesa', qntDespesa],
        ]"
      ></pie-chart>
    </div>
    <div id="grafico2">
      <h1>Status Lançamento</h1>
      <pie-chart
        :data="[
          ['Pendende', qntPendende],
          ['Confirmado', qntConfirmado],
          ['Negativo', qntNegativo],
        ]"
      ></pie-chart>
    </div>
    <div id="grafico3">
        <h1>Valor Total Receitas | Despesas</h1>
    <bar-chart
      :data="[
        ['Valor Total Receitas R$', totalReceitas],
        ['Valor Total Despesas R$', totalDespesas],
      ]"
    ></bar-chart>
    </div>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      configuration: {
        headers: {
          Token: this.$store.state.portarias_token,
        },
      },
      usuario: this.$store.state.usuario,
      lancamentosUsuario: [],
      somaValores: [],
      qntReceita: 0,
      qntDespesa: 0,
      qntConfirmado: 0,
      qntNegativo: 0,
      qntPendende: 0,
      totalReceitas: 0,
      totalDespesas: 0,
    };
  },
  created() {
    this.listaLancamentosUsuario();
  },
  methods: {
    listaLancamentosUsuario() {
      let rota = `lancamentos/usuario/${this.usuario.id}`;
      let qntReceita = 0;
      let qntDespesa = 0;
      let qntConfirmado = 0;
      let qntNegativo = 0;
      let qntPendende = 0;
      let total = 0;
      let totalReceitas = 0;
      let totalDespesas = 0;

      this.axios.get(rota, this.configuration).then((res) => {
        this.lancamentosUsuario = res.data.lancamentos;
        for (let pro of this.lancamentosUsuario) {
          if (pro.tipo_lancamento == "Receita") {
            this.qntReceita = qntReceita += 1;
            this.totalReceitas = totalReceitas += parseFloat(pro.valor);
          } else {
            this.qntDespesa = qntDespesa += 1;
            this.totalDespesas = totalDespesas += parseFloat(pro.valor);
          }
        }
        for (let status of this.lancamentosUsuario) {
          if (status.status == "Negativo") {
            this.qntNegativo = this.qntNegativo += 1;
          } else if (status.status == "Confirmado") {
            this.qntConfirmado = qntConfirmado += 1;
          } else {
            this.qntPendende = this.qntPendende += 1;
          }
        }

        // for(let i of this.lancamentosUsuario){
        //     total += parseFloat(i.valor)
        //     console.log(i.valor)
        // }
        return totalReceitas, totalDespesas;
        console.log("Total Receitas" + totalReceitas);
        console.log("Total Despesas" + totalDespesas);
        // console.log("Conf" + this.qntConfirmado);
        // console.log("Conf" + this.qntNegativo);
        // console.log("Quantidade Lançamentos" + this.lancamentosUsuario.length);
      });
    },
  },
};
</script>
<style>
#grafico1 {
  position: absolute;
  margin-left: 100px;
}
#grafico2 {
  margin-left: 500px;
}
#grafico3{
    margin-top: 100px;
}
</style>
