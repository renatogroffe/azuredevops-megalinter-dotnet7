# azuredevops-megalinter-dotnet7
Exemplo de uso da ferramenta open source MegaLinter na análise de código e detecção de problemas de problemas de segurança a partir de um pipeline do Azure DevOps (com uma aplicação em .NET 7). 

Para saber mais sobre o MegaLinter acesse: https://megalinter.io/latest/

---

Ao executar no pipeline o commando:

```bash
npx mega-linter-runner --flavor dotnet \
    -e "'ENABLE_LINTERS=CSHARP_ROSLYNATOR,REPOSITORY_GITLEAKS,REPOSITORY_SEMGREP'" \
    -e 'SARIF_REPORTER=true' \
    -e 'SHOW_ELAPSED_TIME=true' \
    -e "'REPOSITORY_SEMGREP_RULESETS=p/dockerfile,p/owasp-top-ten'"
```

A imagem **oxsecurity/megalinter-dotnet** será baixada:

![Imagem sendo baixada](img/pipeline-01.png)

Com a execução na sequência do comando (não declarado no pipeline de exemplo, mas acionado a partir de **npx mega-linter-runner**):

```bash
docker run --platform linux/amd64 --rm -v /var/run/docker.sock:/var/run/docker.sock:rw -v /home/vsts/work/1/s/src:/tmp/lint:rw -e ENABLE_LINTERS=CSHARP_ROSLYNATOR,REPOSITORY_GITLEAKS,REPOSITORY_SEMGREP -e SARIF_REPORTER=true -e SHOW_ELAPSED_TIME=true -e REPOSITORY_SEMGREP_RULESETS=p/dockerfile,p/owasp-top-ten oxsecurity/megalinter-dotnet:v8
```

![Execução de container](img/pipeline-02.png)

Produzindo como resultados:

![Resultado no terminal](img/pipeline-03.png)

Na seção **Scans** dos resultados do pipeline constarão os seguintes resultados (ao executar as ferramentas **gitleaks** e **Semgrep**):

![Resultados na aba Scans 1](img/pipeline-04.png)

![Resultados na aba Scans 2](img/pipeline-05.png)

Já em **Artifacts** teremos os seguintes arquivos:

![Resultados em Artifacts 1](img/pipeline-06.png)

![Resultados em Artifacts 2](img/pipeline-07.png)

E no print a seguir temos os logs da análise do código **.NET/C#**:

![Resultados da análise do código .NET/C#](img/pipeline-08.png)