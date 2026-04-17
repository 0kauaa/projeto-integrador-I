# Projeto Integrador I — ANÁLISE DAS NOTIFICAÇÕES DE ESTUPRO E ESTUPRO DE VULNERÁVEL EM SANTOS: UMA ABORDAGEM DE CIÊNCIA DE DADOS SOBRE A DINÂMICA DOS CASOS DE 2022 A 2025.

Estudo derivado de um projeto academico, da matéria de Projeto Integrador I do curso de Ciência de dados da Fatec Baixada Santista "Ruben Lara". O estudo teve como objetivo analisar a relação entre as ocorrências de estupro (Art. 213 do Código Penal) e estupro de vulneravel (Art. 217-A do Código Penal) e suas notificações nos bairros de Santos - SP.

---

## Motivação

O estudo se deu inicio com a intenção de analisar geograficamente os casos de estupro em Santos

O aprendizado de máquina supervisionado é, na prática, tratado de forma procedural: propagar a entrada, calcular o erro, propagar o gradiente, atualizar os parâmetros... Essa visão descreve *como* um modelo aprende, mas não *o que* o aprendizado é.

O artigo *Backprop as a Functor* oferece uma resposta precisa: um algoritmo de aprendizado é um **morfismo** em uma categoria. Nessa formulação, o algoritmo de retropropagação não é implementado explicitamente, mas emerge como consequência direta das leis de composição da categoria **Learn,** estrutura adjacente aos modelos supervisionados.

Este projeto implementa essa estrutura em Haskell e a expande na construção em alguns modelos (atualmente ainda sendo testados), verificando empiricamente que o *backpropagation* é um fenômeno composicional.

---

## A categoria Learn

A categoria **Learn**, formulada em *Bakcprop as a Functor*, é a definida como a estrutura por trás do aprendizado supervisionado, construída sob a perspectiva da Teoria das Categorias. Seus morfismos são uma quadrupla $(P, I, U, r)$ onde:

* $P$ — espaço de parâmetros
* $I : P \times A \to B$ — função de predição ( *implement* )
* $U : P \times A \times B \to P$ — atualização de parâmetros ( *update* )
* $r : P \times A \times B \to A$ — propagação do gradiente ( *request* )

A composição de dois morfismos $f : A \to B$ e $g : B \to C$ define naturalmente o *backpropagation*: o gradiente calculado em $g$ é propagado para $f$ via $r_g$, gerando os dados de treinamento locais para a camada anterior. Essa estrutura formaliza as etapas presentes no processo de aprendizado como transformações de dados componíveis e o treinamento de um modelo como o resultado das composições na estrutura.

---

## Estrutura do projeto

```
.
├── app/                              ← módulos executáveis de cada modelo
│   ├── linear-regressor/             ← regressão linear simples
│   │   └── Main.hs
│   ├── logistic-regressor/           ← regressão logística (sigmoid composto)
│   │   └── Main.hs
│   ├── polynomial-regressor/         ← regressão polinomial de grau 2
│   │   └── Main.hs
│   └── standard-regressor/           ← regressão linear com padronização z-score
│       └── Main.hs
│
├── src/                              ← biblioteca principal
│   ├── Core/
│   │   ├── Cat.hs                    ← typeclass Cat (identidade e composição)
│   │   ├── Learner.hs                ← morfismo Learner como instância de Cat
│   │   └── Params.hs                 ← espaço de parâmetros
│   │
│   ├── Data/
│   │   └── Synthetic/                ← dados sintéticos para treinamento e teste
│   │       ├── Classified.hs         ← dados para classificação binária
│   │       ├── Linear.hs             ← dados gerados sob estrutura linear com ruído
│   │       └── Polynomial.hs         ← dados gerados sob estrutura polinomial com ruído
│   │
│   ├── Models/                       ← morfismos de aprendizado implementados
│   │   ├── LinearRegressor.hs        ← learner da reta ŷ = wx + b
│   │   ├── LogisticRegressor.hs      ← learner logístico (linear composto com sigmoid)
│   │   ├── PolynomialRegressor.hs    ← learner polinomial (linear composto com ajuste quadrático)
│   │   └── StandardRegressor.hs      ← learner com padronização z-score pré-composta
│   │
│   └── Training/
│       └── Training.hs               ← loop de treinamento e utilitários de debug
│
├── test/                             ← verificação empírica das leis categoriais
│   └── Testes.hs                     ← identidade à esquerda, direita e associatividade
│
├── estudo/                           ← experimentos e rascunhos (fora do build)
├── package.yaml
├── stack.yaml
├── stack.yaml.lock
└── README.md
```

---

## **Modelos implementados**

Até então, todos os modelos seguem o funtor $\mathcal{L}_{\varepsilon,e}$ com erro quadrático. O *backpropagation* emerge da composição em todos os casos, sem implementação explícita.

### Regressão linear simples

```haskell
linearRegressor :: Learner '[Double, Double] Double Double
```

Aprende os coeficientes $w$ e $b$ da reta $\hat{y} = wx + b$ por descida de gradiente.

### Regressão com padronização pré-composta

```haskell
standarlizedRegressod :: Learner '[Double, Double] Double Double
standardlizedRegressor = regressorLinear . padronizador mu sigma
```

Um *learner* de padronização $z$-score sem parâmetros aprendíveis é composto com o regressor linear. Demonstra que diferentes algoritmos podem ser compostos
mantendo a capacidade de aprender, e resolve a anisotropia da superfície de perda presente no primeiro modelo.

### Regressão polinomial

```haskell
regressorPolinomial :: Learner '[Double, Double] Double Double
regressorPolinomial = regressorLinear . padronizador mu sigma . ajusteQuadratico
```

Um *learner* que computa $x \mapsto x^2$ sem parâmetros é composto ao regressor padronizado, capturando não-linearidades sem alterar a estrutura categorial.

---

## Como executar

```bash
# clonar o repositório
git clone https://github.com/0kauaa/IC
cd IC/scripts

# compilar
stack build

# rodar os modelos
stack exec linear-regressor
stack exec standard-regressor
stack exec polynomial-regressor

# rodar os testes empíricos das leis categoriais
stack exec testes
```

---

## Referências

* **Fong, B.; Spivak, D. I.; Tuyéras, R.** *Backprop as Functor: A Compositional Perspective on Supervised Learning.* LICS, 2019. [arXiv:1711.10455](https://arxiv.org/abs/1711.10455).
* **Lane, S. M.;  Elinberg S.** *General Theory of Natural Equivalences. American Mathematical Society*, 1948. [https://www.ams.org/journals/tran/1945-058-00/S0002-9947-1945-0013131-6/S0002-9947-1945-0013131-6.pdf]().
* **Rumelhart, D. E.; Hinton, G. E.; Williams, R. J.** *Learning representations by back-propagating errors.* Nature, 323, 533–536, 1986.
* **Mac Lane, S.** *Categories for the Working Mathematician.* 2. ed. Springer, 1998.
* **Goodfellow, I.; Bengio, Y.; Courville, A.** *Deep Learning.* MIT Press, 2016.

---

## Autores

**Kauã Santana da Silva** — discente, FATEC Baixada Santista Rubens Lara
**Alexandre Garcia de Oliveira** — orientador, FATEC Baixada Santista Rubens Lara
