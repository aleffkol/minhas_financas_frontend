<template>
  <div
    style="
      max-width:500px;
      margin: 20% auto;
      border-radius: 10px;
      padding: 40px;
      border: 1px solid #d9d9d9;
      background: #fff;
      margin-top: 80px;
    "
  >
    <a-form
      :form="form"
      :label-col="{ span: 5 }"
      :wrapper-col="{ span: 12 }"
      @submit="handleSubmit"
    >
      <a-form-item label="Nome">
        <a-input
<<<<<<< HEAD
=======
          placeholder="Nome"
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
          v-decorator="[
            'nome',
            { rules: [{ required: true, message: 'Campo Necessario !' }] },
          ]"
        />
      </a-form-item>

      <a-form-item label="Usuario">
        <a-input
<<<<<<< HEAD
=======
          placeholder="Usuario"
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
          v-decorator="[
            'usuario',
            {
              rules: [{ required: true, message: 'Campo Usuario Necessario !' }],
            },
          ]"
        />
      </a-form-item>
      <a-form-item label="Senha">
        <a-input
          v-decorator="[
            'password',
            {
              rules: [
                { required: true, message: 'Por favor insira sua senha !' },
<<<<<<< HEAD
=======
                { min: 6, message: 'Senha precisa de no minimo 6 caracteres.' },  
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
              ],
            },
          ]"
          type="password"
<<<<<<< HEAD
          placeholder="Password"
=======
          placeholder="Senha"
>>>>>>> 6333092217828bec68c7ce769f658510e717b49f
        >
        </a-input>
      </a-form-item>

      <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
        <a-button type="primary" html-type="submit">
          Submit
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formLayout: "horizontal",
      form: this.$form.createForm(this, { name: "coordinated" }),
    };
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
            this.axios.post("usuarios",{
                nome: values.nome,
                usuario: values.usuario,
                senha: values.password
            }).then((res) => {
                this.$router.push({ name: "Login" });
            })
        //   console.log("Received values of form: ", values);
        }
      });
    },
  },
};
</script>
