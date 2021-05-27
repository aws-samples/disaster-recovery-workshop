---
title: "Exponential Backoff"
menutitle: "Exponential Backoff"
weight: 3
chapter: false
pre: "<b>3. </b>"
type: ["Documentation"]
---



Vários componentes em uma rede, como servidores DNS, switches, balanceadores de carga e outros, podem gerar erros em qualquer lugar do ciclo de vida de determinada solicitação. A técnica usual para lidar com essas respostas de erro em um ambiente de rede é implementar novas tentativas no aplicativo cliente. Essa técnica aumenta a confiabilidade do aplicativo e reduz os custos operacionais para o desenvolvedor.

Além de novas tentativas simples, cada SDK da AWS implementa um algoritmo de recuo exponencial para um melhor controle de fluxo. A ideia por trás do backoff exponencial é usar esperas progressivamente mais longas entre as novas tentativas para respostas de erro consecutivas. Você deve implementar um intervalo máximo de atraso, bem como um número máximo de novas tentativas. O intervalo de atraso máximo e o número máximo de novas tentativas não são necessariamente valores fixos, e deve ser definido com base na operação que está sendo executada, assim como outros fatores locais, como a latência da rede.

A maioria dos algoritmos de recuo exponencial usam variação (atraso randomizado) para evitar colisões sucessivas. Como você não está tentando evitar tais colisões nesses casos, não precisa usar esse número aleatório. No entanto, se você usar clientes simultâneos, a variação pode ajudar suas solicitações a serem bem-sucedidas mais depressa. Para obter mais informações, consulte a postagem no blog sobre [Recuo exponencial e variação](https://aws.amazon.com/pt/blogs/architecture/exponential-backoff-and-jitter/).


---
**Referências:**
- [Repetições de erro e recuo exponencial na AWS](https://docs.aws.amazon.com/pt_br/general/latest/gr/api-retries.html)
