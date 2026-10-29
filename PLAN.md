# AI Engineer Path — 12 Haftalık Uygulamalı Plan (Kasım 2026 – Ocak 2027)

## Context

- **Başlangıç noktası:** Frontend developer, JS/TS rahat, Python/Go paslı, AWS ~sıfır. İş tarafı AI engineering'e kayıyor (Claude Code, plugin, workflow ve agent'larla zaten her gün çalışıyorsun; bu büyük bir avantaj).
- **Hedef sırası:** (1) yan gelir → (2) AI engineer rolü → (3) kendi ürünün.
- **Zaman:** hafta içi 2-3 saat (~12 saat/hafta), hafta sonu isteğe bağlı "proje günü".
- **Öğrenme tarzın:** önce video/cheatsheet → not → bilgisayarda yaparak. Uzun doküman okumak yok.
- **Plan bitince:** n8n ve kod tabanlı (TS) workflow/orchestrator kurabilen, bunları AWS'e deploy eden, MCP ile Claude'a bağlayan, eval/observability ekleyen ve bunu satabilen biri olacaksın.

## Neden bu sıra? (araştırmadan çıkanlar)

1. **Piyasa sinyali:** Upwork'ün 2026 raporuna dayandırılan veriler AI entegrasyon/otomasyon talebinin yaklaşık 2 kat arttığını söylüyor. Satılan işler n8n/agent otomasyonu, kendi bilgi tabanıyla çalışan RAG chatbot ve KOBİ workflow tasarımı. "Siteye chatbot widget'ı koymak" ise düşük marjlı. Rakamlar blog kaynaklı, kesin değil, ama yön tutarlı. → **n8n ile başla, en hızlı gelir yolu bu.**
2. **2026 teknoloji durumu:**
   - n8n 2.x: native LangChain node'ları, AI Agent node'u, MCP Client Tool, n8n'in kendi MCP server'ı (Claude bir n8n instance'ına bağlanıp workflow çalıştırabiliyor/üretebiliyor).
   - TS dünyası: Vercel AI SDK (1.0, en çok indirilen) ve Mastra (1.0, AI SDK üstüne agents + workflows + memory + evals). JS'in sayesinde Python'a mecbur değilsin.
   - Claude Agent SDK (`@anthropic-ai/claude-agent-sdk`, eski adı Claude Code SDK) ve MCP TS SDK (`@modelcontextprotocol/sdk`).
   - AWS: Bedrock AgentCore (Runtime + Gateway: Lambda'yı MCP tool'a çeviriyor + Memory), AWS MCP Server (2026'da GA).
   - Eval/observability: Langfuse (tracing, self-host edilebilir) + promptfoo (CI'da eval/red-team; 2026'da OpenAI satın aldı, hâlâ MIT).
3. **İş ilanlarındaki ortak beceri seti:** tool calling, agent loop, multi-agent orchestration (supervisor/worker, graph), MCP, RAG, evals, observability, maliyet optimizasyonu, prompt injection savunması. Plan bu listeyi sırayla kapsıyor.

> Not: Kaynakların çoğu ikincil bloglar. Versiyon/tarih detaylarını ilgili haftada resmi changelog'dan bir kez kontrol et (bu da görevin bir parçası).

## Çalışma yöntemi: "Watch → Cheatsheet → Build → Ship"

Senin yöntemini koruyup iki şey ekliyorum: **kendi cheatsheet'ini üretmek** ve **her hafta bir şey yayınlamak**.

**Günlük 2.5 saatlik seans şablonu:**

| Süre | Ne | Kural |
|---|---|---|
| 20-25 dk | **Watch:** video / kısa kurs bölümü | 1.25-1.5x hız, not almadan izle |
| 10-15 dk | **Cheatsheet:** izlediğini kendi cümlelerinle yaz | `notes/<hafta>/cheatsheet.md`, en fazla 1 sayfa |
| 90 dk | **Build:** günün görevi | Kodu kendin yaz. Claude Code'u **öğretmen modunda** kullan (aşağıya bak) |
| 10 dk | **Log:** `LOG.md`'ye 3 satır (ne yaptım / takıldığım yer / yarın) + commit | Her gün commit |

**Claude'u öğretmen olarak kullan, kod yazdırıcı olarak değil.** İlk 8 hafta için öğrenme reposunun `CLAUDE.md`'sine şunu koy:
> "Ben öğreniyorum. Kodu benim yerime yazma. Sorduğumda kavramı 5 cümlede anlat, bir ipucu ver, yazdığım kodu review et. Ben 'çöz' diyene kadar tam çözüm verme."

Bu kural kaslanmayı sağlar. Mülakatta ve müşteri karşısında "Claude yazdı" değil, "ben kurdum" diyebilmen gerekiyor.

**Haftalık ritim:**
- Pzt–Per: Build günleri (aşağıdaki Gün 1-4)
- Cuma: Gün 5, yani **Ship günü**: haftanın çıktısını bitir, README + 1 dk'lık Loom/GIF, LinkedIn/X'e kısa post ("Bu hafta X kurdum, şunu öğrendim"). Bu postlar yan gelir için hem portföy hem müşteri kanalı.
- Hafta sonu (opsiyonel): proje günü / geride kalanı yakalama. **Plan hafta sonu çalışmaya dayanmıyor.**

**Repo yapısı (1. gün kurulur):** `~/learning/ai-engineer-journey/` → `week-01/ … week-12/`, `notes/`, `LOG.md`, `CLAUDE.md`. Public GitHub, yani portföyün bu.

**İzolasyon (diğer projeler etkilenmez):**
- Repo iş projelerinden (`~/falcon`, `~/optimus-*`, `~/revops-ai` vb.) ayrı bir klasörde, `~/learning/` altında duruyor.
- "Öğretmen modu" kuralı **yalnızca** bu reponun kendi `CLAUDE.md`'sinde yer alıyor. Claude Code bu dosyayı sadece o klasörde (ve alt klasörlerinde) çalışırken okur.
- `~/.claude/CLAUDE.md` (global) ve iş repolarının `CLAUDE.md`'leri hiç değiştirilmiyor. İş projelerinde Claude kod yazmaya eskisi gibi devam eder.
- Öğrenme oturumlarını her zaman `cd ~/learning/ai-engineer-journey && claude` ile başlat.

## Güvenlik ve hukuk ön koşulları (1. gün, 30 dk)

- **Kişisel hesaplar kullan:** kişisel AWS hesabı, kişisel Anthropic API key, kişisel GitHub. Insider SSO/profilleri ve şirket kodu/verisi bu repoya asla girmez.
- **AWS maliyet kalkanı:** hesabı açtığın an root'a MFA, günlük kullanım için IAM Identity Center kullanıcısı, **AWS Budgets $10 ve $25 alarmları**. Yeni hesaplar 2025'ten beri kredi tabanlı free plan ile geliyor; açılışta kredi miktarını kontrol et.
- **Yan gelir:** freelance işe başlamadan önce iş sözleşmendeki yan iş / fikri mülkiyet maddesine bir bak (Hafta 3'ten önce).

---

## FAZ 1 — Temel + İlk Satılabilir Beceri (Hafta 1-3)

### Hafta 1 — LLM API temelleri, TypeScript ile
**Watch:** Anthropic Academy (anthropic.skilljar.com) → "Building with the Claude API" kursunun video bölümleri (ücretsiz, sertifikalı).
**Cheatsheet konuları:** messages API, system prompt, token/maliyet, temperature, streaming, tool use döngüsü, structured output (JSON), prompt caching.
- Gün 1: Repo + CLAUDE.md + budget/hesaplar. Node+TS projesi, `@anthropic-ai/sdk` ile ilk çağrı. Token ve maliyeti konsola yazdır.
- Gün 2: Streaming CLI chat (konuşma geçmişini kendin tut, yani "memory" aslında array'den ibaret, bunu gör).
- Gün 3: **Tool use'u elle yaz:** 2 tool (hava durumu + hesap makinesi), `tool_use` → çalıştır → `tool_result` döngüsü. Framework yok. Agent'ın özünü burada anlarsın.
- Gün 4: Structured output: bir e-postadan `{müşteri, talep, aciliyet}` çıkaran extractor + Zod ile doğrulama. Prompt caching'i aç, maliyet farkını ölç.
- Gün 5 (Ship): `mini-agent` CLI. README'de "agent loop nedir" çizimi.
**Bitti sayılır:** Framework olmadan, sıfırdan çalışan bir tool-calling döngüsü yazabiliyorsun.

### Hafta 2 — n8n temelleri + kendi "AI haber takip" sistemin
**Watch:** n8n'in resmi YouTube kanalı ve n8n Academy'deki beginner/advanced course videoları.
**Cheatsheet:** trigger türleri (webhook, cron, RSS), node'lar arası veri (items, expressions `{{ $json }}`), Code node (JS yazabiliyorsun, avantajın bu), credentials, error workflow.
- Gün 1: n8n'i **local Docker**'da kur (Postgres ile, docker-compose). İlk webhook → Code node → cevap.
- Gün 2-3: **Takip sistemi projesi:** aşağıdaki RSS kaynaklarını çek → tekrarları ele → Claude ile özetle ve "benim için önemi" skorla → haftalık digest'i e-posta/Telegram/Slack'e gönder. (Bölüm sonundaki "Takip sistemi"ne bak.)
- Gün 4: Error workflow + retry + rate limit. Workflow JSON'unu git'e export et.
- Gün 5 (Ship): Digest'in kendisi + workflow JSON + ekran kaydı.
**Bitti sayılır:** Her pazartesi sabahı sana kendi kurduğun AI digest'i geliyor. Teknoloji takibini artık bu yapıyor.

### Hafta 3 — n8n AI Agent node + MCP + satılabilir 2 şablon
**Watch:** n8n AI Agent / MCP videoları (resmi kanal) + DeepLearning.AI "MCP: Build Rich-Context AI Apps with Anthropic" (~1.5 saat, video + notebook; hafta boyunca ilk 20 dakikalara böl).
**Cheatsheet:** AI Agent node (Tools Agent), chat memory, tool olarak sub-workflow, MCP Client Tool, MCP Server Trigger, human-in-the-loop onayı.
- Gün 1: AI Agent node + 2 tool (Google Sheets, HTTP). Memory ekle.
- Gün 2: **Şablon A: Lead qualification:** form/webhook → Claude ile şirket araştır ve skorla → CRM'e (Sheets/HubSpot free) yaz → sıcak lead'i Slack'e bildir.
- Gün 3: **Şablon B: Gelen kutusu triage / fatura çıkarma:** Gmail → sınıflandır → PDF faturadan structured veri → Sheets → onay gerektirenler için insan onayı adımı.
- Gün 4: n8n'i **MCP server** olarak aç, Claude Desktop/Claude Code'dan bir workflow'u tool olarak çağır. (Native MCP özelliğinin güncel durumunu n8n changelog'dan kontrol et.)
- Gün 5 (Ship): 2 şablon için kısa "case study" (problem → çözüm → harcanan süre/kazanç tahmini). **İlk teklif metnini yaz** ("KOBİ'ler için 1 haftada AI otomasyonu").
**Bitti sayılır:** Bir müşteriye gösterebileceğin 2 demo ve 1 teklif metni hazır.

---

## FAZ 2 — AWS (Hafta 4-5)
AWS'i "sertifika ezberi" olarak değil, **kendi n8n'ini ve agent'larını taşıyacak altyapı** olarak öğreneceksin.

### Hafta 4 — AWS çekirdek: IAM, S3, Lambda, API Gateway
**Watch:** AWS Skill Builder'daki ücretsiz "Cloud Quest"/lab'lar ve AWS'nin resmi YouTube kanalındaki servis başına 10 dakikalık videolar (S3, Lambda, IAM). freeCodeCamp'in Cloud Practitioner videosunu yalnızca ilgili bölümler için kullan (hepsini izleme).
**Cheatsheet:** region/AZ, IAM user vs role vs policy (en önemli konu), S3 bucket/object/presigned URL/lifecycle, Lambda (handler, cold start, timeout, env vars), API Gateway (HTTP API), CloudWatch Logs, maliyet modeli.
- Gün 1: Identity Center kullanıcısı + `aws configure sso` (iş yerinde gördüğün akışın aynısı, bu sefer kendi hesabında). AWS CLI ile S3 bucket oluştur, dosya at, presigned URL üret.
- Gün 2: Konsoldan ilk Lambda (Node.js): S3'e dosya düşünce tetiklenen ve dosyayı Claude ile özetleyen fonksiyon. CloudWatch'ta logları izle.
- Gün 3: Aynısını **IaC ile** kur. TS bildiğin için **AWS CDK (TypeScript)** veya **SST** kullan (öneri: CDK, iş ilanlarında daha çok geçiyor). Secrets'ı SSM Parameter Store/Secrets Manager'dan oku.
- Gün 4: API Gateway (HTTP API) + Lambda → `POST /summarize`. DynamoDB'ye istek logu yaz. `cdk destroy` ile her şeyi sil ve tekrar kur.
- Gün 5 (Ship): "S3 → Lambda → Claude" mimari diyagramı + repo.
**Bitti sayılır:** Konsola girmeden, tek komutla kurup silebildiğin bir serverless AI servisi var.

### Hafta 5 — n8n'i production gibi AWS'e taşı + event-driven yapı
**Watch:** EC2/Docker deploy videoları; SQS/EventBridge kısa videoları.
**Cheatsheet:** EC2, security group, Elastic IP, Caddy ile HTTPS, Postgres (RDS vs container), n8n queue mode (Redis + worker), S3 yedek, SQS, EventBridge, Step Functions'a giriş.
- Gün 1: EC2 (küçük instance) + Docker Compose: n8n + Postgres + Caddy (otomatik HTTPS, kendi domainin). Security group'u yalnızca 80/443 açacak şekilde ayarla.
- Gün 2: Günlük Postgres dump'ını ve workflow export'unu **S3'e yedekleyen** cron (lifecycle ile 30 gün sonra Glacier'a geçsin). Restore'u gerçekten dene.
- Gün 3: Queue mode: Redis + 1 worker. (n8n'in binary data için S3 desteğinin lisans gereksinimini resmi dokümandan kontrol et; kaynaklar çelişiyor.)
- Gün 4: **Event-driven:** n8n webhook → SQS → Lambda worker → sonuç S3'e. "Neden kuyruk?" sorusunu cheatsheet'e yaz (retry, backpressure, DLQ).
- Gün 5 (Ship): "Müşteriye self-hosted n8n kurulumu" hizmet paketinin teknik dokümanı. **Bu ayrı satılabilir bir hizmet** (KVKK/GDPR nedeniyle veriyi kendi sunucusunda tutmak isteyen müşteriler).
**Opsiyonel sertifika:** AWS Certified AI Practitioner veya Cloud Practitioner. 12. haftadan sonra, istersen.

---

## FAZ 3 — Claude ekosistemi derinlemesine: MCP + Agent SDK (Hafta 6-7)

### Hafta 6 — Kendi MCP server'ını yaz ve yayınla
**Watch:** Anthropic Academy → "Introduction to MCP" ve "MCP Advanced" kursları.
**Cheatsheet:** MCP mimarisi (host/client/server), tools vs resources vs prompts, transport'lar (stdio, Streamable HTTP), auth (OAuth), tool açıklaması yazma sanatı, güvenlik (tool poisoning, aşırı yetki).
- Gün 1: `@modelcontextprotocol/sdk` ile stdio MCP server: 3 tool. Claude Code'a bağla (`claude mcp add`). MCP Inspector ile test et.
- Gün 2: Gerçek bir niş seç (öneri: Türk KOBİ'lerine yönelik, örneğin e-fatura/kargo takibi/TCMB kur API'si, ya da çok kullandığın bir SaaS) ve onun MCP server'ını yaz.
- Gün 3: Streamable HTTP transport'a geç → **Lambda + API Gateway'e deploy et** (Hafta 4 bilgisiyle). API key ile basit auth.
- Gün 4: Tool açıklamalarını iyileştir; Claude'un yanlış tool seçtiği 5 vakayı topla ve düzelt. npm'e publish et.
- Gün 5 (Ship): npm paketi + README + demo GIF. MCP registry/dizinlerine ekle.
**Bitti sayılır:** Başkalarının `npx` ile kurabildiği, yayında bir MCP server'ın var.

### Hafta 7 — Claude Agent SDK + Claude Code genişletme
**Watch:** Anthropic Academy → "Introduction to Agent Skills" + Agent SDK videoları. Ayrıca Anthropic'in "Building effective agents" yazısı: **bu planda okuman gereken tek uzun metin bu**. Kısa ve görsel, iki seansa böl.
**Cheatsheet:** Agent SDK `query()`, allowed tools, permission modes, in-process MCP, subagent'lar, hooks, skills, session/resume, maliyet kontrolü.
- Gün 1: Agent SDK ile "repo analiz agent'ı": bir klasörü okuyup rapor üretsin. Allowed tools'u kısıtla.
- Gün 2: Kendi MCP server'ını (Hafta 6) Agent SDK'ya bağla. Hooks ile her tool çağrısını logla.
- Gün 3: **Bir Claude Code skill + slash command yaz** (işte her gün kullandığın plugin dünyasının nasıl yapıldığını içeriden gör). Kendi öğrenme reposun için `/log` skill'i: günlük LOG.md girdisini senin için hazırlasın.
- Gün 4: Agent SDK agent'ını bir Lambda'ya veya küçük bir container'a koy, HTTP ile tetikle.
- Gün 5 (Ship): "Agent SDK ile 1 günde dahili otomasyon agent'ı" yazısı.
**Bitti sayılır:** Agent'ları framework ile kurabiliyor, Claude Code'u kendi skill/hook/MCP'lerinle genişletebiliyorsun.

---

## FAZ 4 — Orchestration: asıl hedef (Hafta 8-9)

### Hafta 8 — Orchestration pattern'leri: önce elle, sonra framework ile
**Watch:** AI Engineer YouTube kanalındaki (AI Engineer World's Fair/Summit konuşmaları) agent/orchestration konuşmalarından 3 tane + DeepLearning.AI "AI Agents in LangGraph" (ya da Mastra'nın workflow videoları).
**Cheatsheet:** 5 pattern: prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer. Ayrıca: agent vs workflow farkı, state, checkpoint, human-in-the-loop, ne zaman multi-agent **kullanmamalı**.
- Gün 1: 5 pattern'i **framework'süz TS** ile, 5 küçük dosya olarak yaz (Hafta 1'deki tool loop'u yeniden kullan).
- Gün 2: Aynı orchestrator-workers'ı **Mastra** ile yaz (agents + workflows + memory). İkisini karşılaştır, cheatsheet'e "framework bana ne kazandırdı" yaz.
- Gün 3: Supervisor + 3 uzman sub-agent (araştırmacı, yazar, eleştirmen) → "rakip analizi raporu" üreten sistem. Paralel çalıştır.
- Gün 4: Human-in-the-loop: kritik adımda onay bekle (Slack butonu ya da n8n onay node'u). Paslı Python'u canlandırmak için aynı supervisor'ı LangGraph (Python) ile **1 seansta** tekrar kur. İlanlarda LangGraph çok geçiyor.
- Gün 5 (Ship): 5 pattern repo'su + "hangi durumda hangi pattern" görseli. Bu görsel LinkedIn'de çok iş görür.

### Hafta 9 — Dayanıklı (durable) orchestration + n8n ↔ kod hibriti
**Watch:** AWS Step Functions kısa videoları + (opsiyonel) Inngest/Temporal tanıtımları.
**Cheatsheet:** uzun süren işler, retry/idempotency, timeout, DLQ, state persistence, n8n hangi durumda yeter, ne zaman kod gerekir.
- Gün 1: Hafta 8'deki multi-agent sistemi **Step Functions** ile orkestre et (her agent bir Lambda). Biri patlayınca retry, sonra fallback.
- Gün 2: Hibrit mimari: n8n "dış dünya entegrasyonu ve tetikleme" katmanı, Mastra/Agent SDK servisi "beyin". n8n bu servisi HTTP/MCP ile çağırsın.
- Gün 3: Bedrock'a giriş: aynı agent'ı Bedrock üzerinden Claude ile çalıştır (kurumsal müşteriler bunu ister: veri AWS'den çıkmıyor). AgentCore Gateway ile bir Lambda'yı MCP tool'una çevir.
- Gün 4: Maliyet ve latency ölç: model seçimi (Haiku ile routing, Opus/Sonnet ile ağır iş), prompt caching, paralellik.
- Gün 5 (Ship): Mimari karşılaştırma yazısı: "n8n vs kod vs Step Functions, ne zaman hangisi".
**Bitti sayılır:** Bir orchestrator'ı hem n8n'de hem kodda kurup AWS'te dayanıklı şekilde çalıştırabiliyorsun. **Planın ana hedefi bu.**

---

## FAZ 5 — Production kalitesi (Hafta 10-11)

### Hafta 10 — RAG + memory
**Watch:** RAG temelleri kısa videoları (DeepLearning.AI short courses), embeddings görsel anlatımları.
**Cheatsheet:** chunking stratejileri, embeddings, vector DB (pgvector, S3 Vectors, managed seçenekler), hybrid search (BM25 + vektör), reranking, contextual retrieval, "RAG mı, uzun context mi?"
- Gün 1: Postgres + pgvector (docker) ile şirket dokümanı RAG'ı: ingest → chunk → embed → search.
- Gün 2: Hybrid search + reranking ekle; iyileşmeyi 20 soruluk bir setle ölç.
- Gün 3: Aynı dokümanları S3'e koy → AWS tarafında managed seçeneği dene (Bedrock Knowledge Bases / S3 Vectors; güncel durumu kontrol et). Karşılaştır.
- Gün 4: **Satılabilir şablon C:** "Kendi bilgi tabanıyla müşteri destek botu": n8n chat trigger + senin RAG servisin + insana devretme. Piyasada en çok talep gören işlerden biri.
- Gün 5 (Ship): Demo + case study.

### Hafta 11 — Evals, observability, güvenlik
**Watch:** Langfuse ve promptfoo'nun kendi YouTube/demo videoları; AI Engineer kanalında "evals" konuşmaları.
**Cheatsheet:** trace/span, LLM-as-judge, golden dataset, regression eval, CI'da eval, prompt injection (direkt/dolaylı), tool yetki sınırlama, PII maskeleme, maliyet dashboard'u.
- Gün 1: Langfuse'u self-host et (docker, sonra EC2'ye), Hafta 8-10 sistemlerine tracing ekle (OpenTelemetry).
- Gün 2: Destek botu için 30 soruluk golden dataset + LLM-as-judge eval'ı.
- Gün 3: promptfoo ile eval'ı **GitHub Actions**'a koy; prompt değişince skor düşerse CI kırmızı olsun. Red-team: kendi botuna 10 prompt injection dene, savunma ekle.
- Gün 4: Maliyet/latency dashboard'u; aylık müşteri başı maliyeti hesapla (fiyatlandırma için şart).
- Gün 5 (Ship): "AI otomasyonlarını nasıl test ediyorum" yazısı. Müşteriye güven satan içerik.
**Bitti sayılır:** Sistemlerinin ne yaptığını görebiliyor, kaliteyi ölçebiliyor ve maliyetini biliyorsun. Seni "n8n'ci"den "AI engineer"a taşıyan fark bu.

---

## FAZ 6 — Capstone + Para (Hafta 12)

### Hafta 12 — Uçtan uca ürün ve satış
- Gün 1-3: **Capstone:** tek bir niş için uçtan uca sistem. Öneri: "e-ticaret KOBİ'si için AI operasyon asistanı" (sipariş/kargo/iade soruları RAG + MCP tool'larıyla, n8n entegrasyonları, AWS'te koşuyor, Langfuse ile izleniyor, eval'lı). Hafta 2-11'deki parçaları birleştirmekten ibaret, sıfırdan yazma yok.
- Gün 4: Portföy sayfası (frontend'cisin, 1 günde çok iyi bir landing page çıkarırsın): 3 case study, 3 paket (Kurulum / Otomasyon / Aylık bakım), fiyat aralıkları.
- Gün 5: Upwork + LinkedIn profili, n8n template gallery'ye şablon gönderimi, ilk 10 kişiye doğrudan mesaj (web/pazarlama ajansları iyi bir kanal; onlara alt yüklenici ol).

**12. haftanın sonunda elinde olanlar:**
- 3 satılabilir n8n şablonu (lead, inbox/fatura, destek botu) + case study'leri
- npm'de yayında 1 MCP server
- AWS'te IaC ile kurulu: self-hosted n8n, serverless AI servisi, Step Functions orchestrator
- 5 pattern'li orchestration repo'su (Mastra + elle yazılmış + LangGraph)
- Eval'lı ve izlenen 1 RAG sistemi + CI
- 12 haftalık public log + 12 LinkedIn postu
- Çalışan kişisel AI haber digest'in

---

## Teknoloji takip sistemi (Hafta 2'de kurulur, sonra kendiliğinden işler)

**Kendi n8n digest'ine bağlanacak kaynaklar (RSS/API):**
- Anthropic news + Claude Code changelog + Claude API release notes
- n8n release notes (GitHub releases)
- AWS "What's New" (yalnızca AI/ML, Lambda, S3, Bedrock etiketleri)
- Simon Willison'ın blogu (en iyi tek kişilik LLM haber kaynağı)
- Latent Space (AI engineer odaklı podcast + newsletter)
- smol.ai AINews (günlük AI haber özeti; digest'in bunu da özetlesin)
- Hacker News: "LLM / agent / MCP" filtreli
- GitHub trending (TypeScript + "agent" / "mcp")

**Video tarafı (senin tarzına en uygun):** AI Engineer YouTube kanalı (konferans konuşmaları, AI engineering'in "resmi" kanalı gibi), Anthropic YouTube, n8n YouTube, AWS Events (re:Invent oturumları, **re:Invent 2026 Aralık başında**: Hafta 5-6'ya denk geliyor, agent/serverless oturumlarını digest'e ekle).

**Haftalık 30 dk "radar" seansı (Cuma, Ship'ten sonra):** digest'ten 3 şey seç → her biri için `notes/radar.md`'ye 2 satır ("ne / bana faydası") → birini sonraki haftanın build'ine küçük bir deney olarak ekle. Planın "benim söylemediğim yeni gelişmeler" kısmı böylece canlı kalır.

## Plana eklediğim, senin söylemediğin konular

- **MCP'nin kendisini yazmak** (sadece kullanmak değil): Claude ekosisteminde en ayırt edici beceri.
- **Claude Agent SDK + Claude Code skill/hook yazmak:** işte kullandığın plugin'lerin nasıl yapıldığını anlarsın; şirket içinde AI engineer'lığa geçişte doğrudan değer üretir.
- **Mastra / Vercel AI SDK:** JS bilgini doğrudan agent geliştirmeye çeviriyor.
- **Bedrock AgentCore + AWS MCP Server:** kurumsal müşteri ve AI engineer ilanları için AWS'in agent katmanı.
- **Evals + observability (Langfuse, promptfoo):** junior/hobi seviyesiyle profesyoneli ayıran konu.
- **Prompt injection ve tool güvenliği:** müşteriye satarken sorulan ilk sorulardan.
- **Maliyet mühendisliği:** model routing, caching. Fiyat verebilmek için gerekli.

## İlerleme kontrolü (verification)

Her faz sonunda kendine bu soruları sor; cevap "hayır" ise bir sonraki fazdan önce hafta sonunu buna ayır:
- **Faz 1:** Framework'süz bir tool loop'u 30 dakikada sıfırdan yazabiliyor musun? n8n'de bir müşteri demosunu 1 saatte kurabiliyor musun?
- **Faz 2:** `cdk deploy` / `cdk destroy` ile servisini kurup silebiliyor musun? Aylık AWS faturan $10'un altında mı?
- **Faz 3:** MCP server'ın npm'de mi ve başka biri kurabiliyor mu?
- **Faz 4:** 5 pattern'i birine beyaz tahtada anlatıp her birine bir müşteri senaryosu verebiliyor musun?
- **Faz 5:** Bir prompt değişikliğinin kaliteyi bozup bozmadığını CI söylüyor mu?
- **Faz 6:** En az 1 ücretli iş teklifi verilmiş mi (kabul edilmese bile)?

**Esneklik kuralı:** Bir hafta kayarsa planı kaydır, sıkıştırma. Gün 5 (Ship) atlanmaz; gerekirse Gün 4'ü kısalt.

## Sonraki adım (onaydan sonra)

İlk iş olarak `~/learning/ai-engineer-journey` reposunun iskeletini (klasörler, `CLAUDE.md` öğretmen kuralı, `LOG.md` şablonu, Hafta 1 görev dosyası) ve 1. haftanın gün gün checklist'ini oluşturabilirim. Ayrıca planı Takvim'e haftalık bloklar olarak işlemeyi önerebilirim; bunu yalnızca sen istersen yaparım.
