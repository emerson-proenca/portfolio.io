Guia gerado pelo Gemini, servi de guia para mim :D
---

# 🚀 Guia de Implementação: Portfólio Interativo

Este guia detalha como transformar um portfólio estático em uma vitrine técnica dinâmica, focada em performance e monitoramento em tempo real.

## 1. Monitor de Atividade (Real-time)

### GitHub - Último Commit

* **API:** `https://api.github.com/users/{USUARIO}/events`
* **O que extrair:** Procure pelo primeiro item com `type: "PushEvent"`.
* **Exibição:** "Último código enviado para [Repositório]: '[Mensagem do Commit]' (há X minutos)".

### LeetCode - Último Problema

* **Abordagem:** Como o LeetCode usa GraphQL e possui CORS restrito, utilize uma bridge como a [leetcode-stats-api](https://github.com/JeremyTsaii/leetcode-stats-api).
* **O que extrair:** Nome do problema, dificuldade e timestamp da submissão.

---

## 2. Playground de Performance (DB & API)

O objetivo é provar que você entende de latência e otimização.

### Lógica de Backend (Pseudo-código Node.js)

```javascript
async function handleQuery(req, res) {
  const { limit } = req.query;
  const cacheKey = `query_limit_${limit}`;

  const start = performance.now();

  // 1. Tentar Cache
  const cachedData = await redis.get(cacheKey);
  
  if (cachedData) {
    const end = performance.now();
    return res.json({
      status: "Cache HIT",
      time: `${(end - start).toFixed(2)}ms`,
      data: JSON.parse(cachedData)
    });
  }

  // 2. Fallback para o Banco de Dados
  const data = await db.query(`SELECT * FROM users LIMIT ${limit}`);
  await redis.set(cacheKey, JSON.stringify(data), 'EX', 60);

  const end = performance.now();
  res.json({
    status: "Cache MISS (DB)",
    time: `${(end - start).toFixed(2)}ms`,
    data
  });
}

```

---

## 3. UI/UX Suggestions

* **Indicadores Visuais:**
* **Verde:** Para `Cache HIT` (Velocidade máxima).
* **Amarelo:** Para `Cache MISS` (Busca no Banco).


* **Gráfico Comparativo:** Um pequeno gráfico de barras comparando o tempo da primeira requisição (MISS) vs a segunda (HIT).
* **Disclaimer de Segurança:** Informe que os parâmetros são higienizados para evitar SQL Injection.

---

## 4. Stack Recomendada

* **Frontend:** Svelte .
* **Database:** Supabase.
* **Hospedagem:** Vercel.

---
