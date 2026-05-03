# 📱 Sistema de Smartphones — Abstração e Herança com POO em C#

![Banner](https://github.com/user-attachments/assets/d8b60a21-f60e-47aa-a3aa-ee9e5582762b)

> **Bootcamp WEX — End to End Engineering · DIO**

[![.NET](https://img.shields.io/badge/.NET-6.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)
[![C#](https://img.shields.io/badge/C%23-Language-239120?style=for-the-badge&logo=csharp&logoColor=white)](https://learn.microsoft.com/dotnet/csharp/)
[![POO](https://img.shields.io/badge/Paradigma-POO-0078D7?style=for-the-badge)](https://en.wikipedia.org/wiki/Object-oriented_programming)
[![Abstração](https://img.shields.io/badge/Conceito-Abstração_e_Herança-9B59B6?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Concluído-00b300?style=for-the-badge)]()

---

## 1. Problema de Negócio

Sistemas que gerenciam múltiplas marcas e modelos de dispositivos enfrentam um problema de escalabilidade clássico: **como adicionar suporte a uma nova marca sem duplicar código** e sem que o comportamento específico de uma marca contamine a lógica comum a todos os dispositivos?

O anti-padrão mais comum é criar classes independentes e não relacionadas para cada marca — o que quebra a capacidade de tratar todos os dispositivos de forma uniforme e obriga a duplicar métodos idênticos como `Ligar()` e `ReceberLigacao()` em cada classe.

Este projeto modela uma solução orientada a objetos que resolve esse problema: define um contrato comum via classe abstrata, compartilha comportamentos genéricos por herança e impõe que cada fabricante implemente seu próprio comportamento de instalação de aplicativos via método abstrato.

---

## 2. Contexto

O sistema foi desenvolvido como desafio de projeto no **Bootcamp WEX — End to End Engineering**, com foco na aplicação dos quatro pilares da POO — **abstração, encapsulamento, herança e polimorfismo** — em um domínio familiar e representativo.

O modelo envolve três classes com hierarquia explícita:

- **`Smartphone`** (abstrata) — define o contrato e os comportamentos comuns a qualquer dispositivo: número, modelo, IMEI, memória, `Ligar()` e `ReceberLigacao()`
- **`Nokia`** (concreta) — herda de `Smartphone` e implementa `InstalarAplicativo()` com o comportamento da Nokia Store
- **`Iphone`** (concreta) — herda de `Smartphone` e implementa `InstalarAplicativo()` com o comportamento da App Store

O cenário de negócio que orienta o design: **Nokia e iPhone compartilham a mesma interface de uso** (qualquer um pode ligar, receber ligação e instalar apps), mas a **forma de instalar um aplicativo é específica de cada fabricante**.

---

## 3. Premissas da Análise

- `Smartphone` é declarada `abstract`, impedindo instâncias diretas — nenhum "smartphone genérico" existe no mundo real; todo dispositivo é de uma marca específica
- `InstalarAplicativo()` é declarado `abstract` na classe base, tornando obrigatória a implementação nas classes filhas — a compilação falha se uma subclasse não implementar o método
- As propriedades `Modelo`, `IMEI` e `Memoria` são `protected` (não `private`), permitindo acesso direto pelas subclasses sem expor ao mundo externo — decisão relevante para futuras subclasses que precisem exibir esses dados
- `Numero` é `public` porque representa o identificador visível do dispositivo para qualquer consumidor do sistema
- O escopo é um console application de demonstração, focado em validar o design da hierarquia de classes

---

## 4. Estratégia da Solução

O desenvolvimento seguiu o processo de modelagem orientada ao domínio, partindo do contrato antes das implementações:

**Passo 1 — Identificar o que é comum e o que é variável**
`Ligar()` e `ReceberLigacao()` são comportamentos idênticos em qualquer smartphone — implementados uma vez na classe base. `InstalarAplicativo()` varia por fabricante — declarado `abstract` para forçar implementação específica.

**Passo 2 — Definir o contrato em `Smartphone`**
A classe abstrata define as propriedades do dispositivo com visibilidade adequada e o construtor que as inicializa. O método `abstract InstalarAplicativo()` cria o contrato que todas as subclasses devem cumprir.

**Passo 3 — Implementar `Nokia` com herança e `override`**
`Nokia` chama `base(numero, modelo, imei, memoria)` no construtor, delegando a inicialização das propriedades para a classe pai. O `override` em `InstalarAplicativo()` injeta o comportamento específico da Nokia Store.

**Passo 4 — Implementar `Iphone` com o mesmo padrão**
Mesma estrutura que `Nokia`, com o comportamento específico da App Store. A uniformidade estrutural entre `Nokia` e `Iphone` demonstra o padrão: adicionar um terceiro fabricante (`Samsung`, por exemplo) requer apenas criar uma nova classe com o mesmo contrato — sem tocar nas classes existentes.

**Passo 5 — Demonstrar polimorfismo em `Program.cs`**
Ambos os objetos são criados, e os mesmos métodos (`Ligar()`, `ReceberLigacao()`, `InstalarAplicativo()`) são chamados em cada um. O comportamento de `InstalarAplicativo()` diverge em tempo de execução conforme o tipo concreto do objeto — polimorfismo em ação.

---

## 5. Insights Técnicos

A modelagem revela decisões de design com impacto direto na extensibilidade e na manutenibilidade:

**`abstract class` vs `interface` para o contrato de Smartphone**
Uma `interface` define apenas assinaturas — nenhum comportamento compartilhado. Uma `abstract class` permite que métodos comuns como `Ligar()` sejam implementados uma vez e herdados por todas as subclasses. A escolha por `abstract class` é correta aqui porque `Smartphone` tem tanto comportamento compartilhado (métodos concretos) quanto comportamento forçado (método abstrato).

**`abstract` vs `virtual` em `InstalarAplicativo()`**
`virtual` permite override mas não obriga — uma subclasse pode herdar o comportamento padrão sem sobrescrever. `abstract` obriga a implementação: se `Nokia` ou `Iphone` não implementarem `InstalarAplicativo()`, o código não compila. Para o domínio de fabricantes de smartphone, onde cada loja de apps é estruturalmente diferente, `abstract` é a escolha correta — não existe comportamento padrão sensato.

**`protected` nas propriedades de Smartphone**
`private` tornaria as propriedades invisíveis para `Nokia` e `Iphone`. `public` as exporia para qualquer consumidor externo. `protected` define o escopo correto: acessível dentro da hierarquia de herança, invisível fora dela. Isso é encapsulamento aplicado com precisão de nível de acesso.

**Construtor com `base(...)`**
`Nokia` e `Iphone` não replicam a lógica de atribuição das propriedades — eles delegam para `base(numero, modelo, imei, memoria)`. Isso garante que se o construtor de `Smartphone` mudar (ex: adicionar validação de IMEI), todas as subclasses herdam a mudança automaticamente.

**Polimorfismo como extensibilidade futura**
Com a arquitetura atual, adicionar suporte a `Samsung` requer apenas criar `Samsung : Smartphone` e implementar `InstalarAplicativo()` com o comportamento da Galaxy Store. Nenhuma das classes existentes precisa ser modificada — o sistema é aberto para extensão e fechado para modificação (Open/Closed Principle).

---

## 6. Resultados

O sistema demonstra os quatro pilares da POO aplicados de forma coesa em um domínio real:

```
✅ Abstração:     Smartphone captura o essencial de qualquer dispositivo
✅ Encapsulamento: Propriedades protected isolam estado interno da hierarquia
✅ Herança:        Nokia e Iphone reusam Ligar() e ReceberLigacao() sem duplicação
✅ Polimorfismo:   InstalarAplicativo() tem comportamentos distintos por fabricante
```

Saída do `Program.cs` ao executar:

```
Smartphone Nokia:
Ligando...
Recebendo ligação...
Instalando o aplicativo "WhatsApp" na loja da Nokia (Nokia Store).

Smartphone iPhone:
Ligando...
Recebendo ligação...
Instalando o aplicativo "Instagram" na App Store (iPhone).
```

![Diagrama de Classes](https://github.com/user-attachments/assets/e4369031-feea-44ef-ad30-f7f941916275)

---

## 7. Próximos Passos

- [ ] Adicionar classe `Samsung` herdando de `Smartphone`, demonstrando que a arquitetura é extensível sem modificar código existente
- [ ] Implementar `interface IConectavel` com métodos `ConectarWifi()` e `ConectarBluetooth()`, introduzindo composição de comportamento via múltiplas interfaces
- [ ] Adicionar validação de IMEI no construtor de `Smartphone` (formato `##-######-######-#`), demonstrando guard clauses em construtores de classes abstratas
- [ ] Criar testes unitários com **xUnit** verificando que `Smartphone` não pode ser instanciado diretamente e que cada subclasse produz a mensagem correta de instalação
- [ ] Evoluir para um catálogo de dispositivos com busca por modelo e fabricante, aplicando coleções genéricas (`List<Smartphone>`) e polimorfismo em tempo de execução

---

## 🗂️ Estrutura do Projeto

```
DesafioPOO/
├── Models/
│   ├── Smartphone.cs   # Classe abstrata: contrato + comportamentos compartilhados
│   ├── Nokia.cs        # Subclasse concreta: InstalarAplicativo via Nokia Store
│   └── Iphone.cs       # Subclasse concreta: InstalarAplicativo via App Store
├── Program.cs          # Demonstração do polimorfismo em tempo de execução
└── DesafioPOO.csproj
```

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Papel no Projeto |
|---|---|---|
| C# / .NET | 6.0 | Linguagem e plataforma de execução |
| POO — Abstração | — | `abstract class` define o contrato do domínio |
| POO — Herança | — | Nokia e Iphone reusam comportamentos de Smartphone |
| POO — Polimorfismo | — | `override` injeta comportamento específico por fabricante |
| POO — Encapsulamento | — | `protected` controla visibilidade dentro da hierarquia |
| Git + GitHub | — | Versionamento e documentação |

---

## ▶️ Como Executar

**Pré-requisito:** [.NET 6 SDK](https://dotnet.microsoft.com/download/dotnet/6) ou superior instalado.

```bash
# Clone o repositório
git clone https://github.com/Santosdevbjj/sistema-POO-celular.git
cd sistema-POO-celular

# Execute a aplicação
dotnet run
```

A saída demonstra os dois fabricantes executando os mesmos métodos com comportamentos distintos em `InstalarAplicativo()`.

---

## 📐 Decisões Técnicas

**Por que `abstract class` e não apenas uma classe comum com métodos virtuais?**
Uma classe comum com métodos virtuais permitiria instanciar `Smartphone` diretamente — o que não faz sentido no domínio: não existe "um smartphone genérico", existe sempre um Nokia ou um iPhone. `abstract` torna essa restrição explícita no contrato da linguagem, não apenas em convenção de time.

**Por que `InstalarAplicativo()` é `abstract` e não `virtual` com implementação padrão?**
Um comportamento padrão de instalação não existe no domínio real — a loja de apps é estruturalmente diferente em cada ecossistema. Fornecer uma implementação padrão em `Smartphone` seria uma decisão arbitrária que poderia ser herdada silenciosamente por novas subclasses, introduzindo bugs sutis. `abstract` força cada novo fabricante a declarar explicitamente seu comportamento.

**Por que o construtor de `Nokia` e `Iphone` delega para `base(...)`?**
Porque a responsabilidade de inicializar as propriedades de `Smartphone` pertence à própria classe `Smartphone`. Replicar essa lógica nas subclasses criaria acoplamento de implementação: se o construtor base mudar, seria necessário atualizar cada subclasse manualmente. Com `base(...)`, a mudança ocorre em um único lugar.

---

## 🧠 Aprendizados

O maior aprendizado foi entender que **abstração não é sobre simplificar** — é sobre **separar o que é contrato do que é implementação**. A classe `Smartphone` não existe para ser usada diretamente: ela existe para definir o que qualquer smartphone deve ser capaz de fazer, sem ditar como cada fabricante faz.

O insight mais valioso veio da diferença entre `abstract` e `virtual`: ambos permitem override, mas apenas `abstract` garante que o override aconteça. Em sistemas reais, essa garantia em tempo de compilação elimina uma categoria inteira de bugs onde uma nova subclasse herda silenciosamente um comportamento incorreto.

O que faria diferente desde o início: incluiria uma lista `List<Smartphone>` no `Program.cs` para demonstrar polimorfismo em coleções — iterar sobre Nokia e Iphone chamando `InstalarAplicativo()` sem saber o tipo concreto é onde o poder da abstração realmente aparece.

---

## 🤝 Contribuição

Sugestões de novos fabricantes, padrões de design ou evoluções de arquitetura são bem-vindos. Abra uma issue ou envie um pull request.

---

## 📬 Contato

[![Portfólio](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
[![GitHub](https://img.shields.io/badge/GitHub-Santosdevbjj-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Santosdevbjj)
