
    <html>
    <head>
    <title>ViaCEP Webservice</title>
		<meta charset="utf-8">
		  <meta name="viewport" content="width=device-width, initial-scale=1">
		  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
		  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

    <!-- Adicionando JQuery -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"
            integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo="
            crossorigin="anonymous"></script>

		<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
		<script>
		$(document).ready(function(){
		  $("p").click(function(){
		    $(this).hide();
		  });
		});
		</script>
		
    <!-- Adicionando Javascript -->
    <script>
    

        $(document).ready(function() {

            function limpa_formulário_cep() {
                // Limpa valores do formulário de cep.
                $("#rua").val("");
                $("#bairro").val("");
                $("#cidade").val("");
                $("#uf").val("");
   
            }
            
            //Quando o campo cep perde o foco.
            $("#cep").blur(function() {

                //Nova variável "cep" somente com dígitos.
                var cep = $(this).val().replace(/\D/g, '');

                //Verifica se campo cep possui valor informado.
                if (cep != "") {

                    //Expressão regular para validar o CEP.
                    var validacep = /^[0-9]{8}$/;

                    //Valida o formato do CEP.
                    if(validacep.test(cep)) {

                        //Preenche os campos com "..." enquanto consulta webservice.
                        $("#rua").val("...");
                        $("#bairro").val("...");
                        $("#cidade").val("...");
                        $("#uf").val("...");
                     

                        //Consulta o webservice viacep.com.br/
                        $.getJSON("https://viacep.com.br/ws/"+ cep +"/json/?callback=?", function(dados) {

                            if (!("erro" in dados)) {
                                //Atualiza os campos com os valores da consulta.
                                $("#rua").val(dados.logradouro);
                                $("#bairro").val(dados.bairro);
                                $("#cidade").val(dados.localidade);
                                $("#uf").val(dados.uf);
                               
                            } //end if.
                            else {
                                //CEP pesquisado não foi encontrado.
                                limpa_formulário_cep();
                                alert("CEP não encontrado.");
                            }
                        });
                    } //end if.
                    else {
                        //cep é inválido.
                        limpa_formulário_cep();
                        alert("Formato de CEP inválido.");
                    }
                } //end if.
                else {
                    //cep sem valor, limpa formulário.
                    limpa_formulário_cep();
                }
            });
        });

    </script>
    </head>

    <body>
 
    <!-- Inicio do formulario -->
      <form method="get" action=".">
    <div class="mb-3 mt-3">
      <label for="cep">Cep:</label>
      <input type="cep" class="form-control" id="cep" placeholder="Cep" name="cep" value="" size="10" maxlength="9">
    </div>
    <div class="mb-3">
      <label for="rua">Rua:</label>
      <input type="text" class="form-control" id="rua" placeholder="Rua" name="rua"size="60">
    </div>
     <div class="mb-3">
      <label for="bairro">Bairro:</label>
      <input type="bairro" class="form-control" id="bairro" placeholder="Bairro" name="bairro" size="40">
    </div>
     <div class="mb-3">
      <label for="cidade">Cidade:</label>
      <input type="text" class="form-control" id="cidade" placeholder="Cidade" name="cidade"size="40">
    </div>
    <div class="mb-3">
      <label for="uf">Estado:</label>
      <input type="text" class="form-control" id="uf" placeholder="UF" name="uf">
    </div>
    <div class="form-check mb-3">
      <label class="form-check-label">
        <input class="form-check-input" type="checkbox" name="remember"> Remember me
      </label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
  
    </body>

    </html>
