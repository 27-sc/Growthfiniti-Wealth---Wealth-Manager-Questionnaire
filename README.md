import React, { useMemo, useState, useEffect } from "react";

const FORMSPREE_ENDPOINT = "https://formspree.io/f/REPLACE_ME";

export default function WealthManagerEvaluationForm() {
  const brand = { teal: "#03333F", green: "#38B87F", gold: "#F5BB57", mist: "#F5F8F7" };
  const MIN_WORDS = 20; // minimum words per answer

  // --- Content: sections + demo tips ---
  const sections = useMemo(() => ([
    {
      key: "philosophy",
      title: "Investment Philosophy & Framework",
      questions: [
        "What is your core investment philosophy?",
        "How do you balance capital preservation with growth?",
        "Do you use proprietary models? If so, how is it implemented in portfolio construction?",
        "How do you quantify and incorporate client views in your model?",
        "How do you ensure the portfolio remains aligned with macroeconomic shifts and valuation dislocations?",
      ],
      tips: [
        "Example: Our core philosophy is long‑term, research‑driven investing. We focus on quality businesses with sustainable cash flows, diversify across sectors/regions, and avoid speculation. We use evidence‑based factors (quality, value, momentum) and rebalance to maintain target exposures.",
        "Example: We preserve capital by position sizing, stop‑loss/hedge rules, and limits on illiquid assets. For growth, we set a strategic equity range (e.g., 45–65%) and opportunistically tilt toward factors with attractive valuations.",
        "Example: We run a proprietary Black‑Litterman framework. Inputs include market caps, covariance from 5‑year data, and our view vectors with confidence. The optimizer outputs weights within guardrails and risk budgets.",
        "Example: Client views are captured via a short survey and meetings. We translate views (e.g., ‘overweight domestic quality banks’) into model inputs with confidence levels so they affect weights proportionally.",
        "Example: We track rates, inflation, earnings breadth, and valuation spreads. Material shifts trigger small, rules‑based tilts (±2–5%) or hedges; valuation dislocations prompt rebalancing toward cheaper assets."
      ],
    },
    {
      key: "risk",
      title: "Risk Management & Risk Budgeting",
      questions: [
        "How do you define and measure risk for a client portfolio?",
        "Do you use risk budgets? If yes, how are they allocated across asset classes or strategies?",
        "How is risk budgeting integrated into portfolio rebalancing decisions?",
        "Can you share a real-life example where risk budgeting helped mitigate drawdowns?",
      ],
      tips: [
        "Example: We define risk as the chance of permanent capital loss. We monitor volatility (stdev), max drawdown, Value‑at‑Risk, and tracking error to the blended benchmark. We also run stress tests (rate shock, currency move).",
        "Example: Yes. We set risk budgets by asset class (e.g., Equity 60%, Fixed Income 30%, Alts 10%) and within equity by factor/sector. No single position >5% and no factor >1.5× benchmark exposure.",
        "Example: If market moves push equity risk above budget, rules trigger partial trims and rebalance back to target bands. Cash from trims is redeployed to under‑budget assets (e.g., bonds) to restore balance.",
        "Example: In Mar 2020, high‑beta names breached risk limits. We rotated into quality/low‑vol ETFs and added IG bonds, cutting peak drawdown ~300 bps versus benchmark while maintaining liquidity."
      ],
    },
    {
      key: "managers",
      title: "Manager Selection & Fund Due Diligence",
      questions: [
        "What is your process for selecting external fund managers?",
        "How do you evaluate consistency and style drift in fund manager performance?",
        "What qualitative and quantitative filters do you apply before selecting a fund?",
        "Do you conduct ongoing reviews of fund managers? How often?",
      ],
      tips: [
        "Example: Process = screen (3–5yr risk/return, fees) → diligence (team, process, capacity) → ops check (custody, compliance) → IC approval → watchlist. We prefer repeatable, disciplined processes.",
        "Example: We test rolling 12/36‑month returns, holdings‑based factor loadings, and active share. If a value manager loads like growth for >2 quarters, that’s style drift and prompts a review.",
        "Example: TER, AUM stability, downside capture, batting average, people/process/price, governance, and ESG policy. We avoid key‑person risk and capacity‑strained strategies.",
        "Example: Quarterly monitoring with an annual deep‑dive. Triggers for action: material team change, persistent underperformance vs style‑correct benchmark, breaches in risk/compliance."
      ],
    },
    {
      key: "customisation",
      title: "Customisation & Client Suitability",
      questions: [
        "How do you tailor strategies to different investor profiles—conservative, balanced, aggressive?",
        "Can you explain how specific goals (e.g., retirement, children’s education) map to your investment approach?",
        "How flexible is your strategy in adapting to evolving financial situations or macro shocks?",
      ],
      tips: [
        "Example: Conservative 20/70/10 (Equity/Debt/Alts), Balanced 50/40/10, Aggressive 70/20/10. We set guardrails per profile and rebalance to ranges semiannually.",
        "Example: Goals map to time‑bucketed sleeves: near‑term (0–3y) cash/short debt; medium (3–7y) balanced; long‑term (7y+) equity‑led with factor tilts. Progress tracked against glidepaths.",
        "Example: We review whenever life events occur (job change, home purchase) and during macro shocks. In 2020 we de‑risked toward quality debt, then gradually re‑risked as indicators normalised."
      ],
    },
    {
      key: "performance",
      title: "Performance & Track Record",
      questions: [
        "What is your firm’s track record over 1 & 3 years (net of fees)?",
        "Can you show performance attribution by asset class and strategy?",
        "How do you benchmark performance—and why?",
      ],
      tips: [
        "Example: Net of fees, 1‑yr = 12.0%; 3‑yr CAGR = 10.1%; information ratio 0.5. Past performance isn’t indicative; numbers audited by external CPA.",
        "Example: 2024 attribution: Equity +8.2% (quality tilt), FI +1.9% (duration extension), Alts +0.7% (REITs). Detractors: small‑cap factor in Q2.",
        "Example: Blended benchmark (e.g., 60% Nifty 500 TR, 40% India G‑Sec 7–10). It mirrors strategic asset mix and avoids misleading single‑index comparisons."
      ],
    },
    {
      key: "governance",
      title: "Experience & Governance",
      questions: [
        "What is your personal experience in wealth management, and how long have you been practicing?",
        "Do you hold any fiduciary responsibility toward your clients?",
        "What is your team’s background in portfolio construction, asset allocation, or quantitative modeling?",
      ],
      tips: [
        "Example: 15 years advising HNI/ULTRA HNI clients; led allocation committees; managed ₹500cr+ AUM; prior roles at global banks and a PMS.",
        "Example: Yes—fiduciary duty with documented conflict policy, best‑execution standards, and fee transparency.",
        "Example: Team includes CFA/FRM charterholders and quants experienced in factor design, BL optimisation, and risk systems."
      ],
    },
    {
      key: "technology",
      title: "Technology & Reporting",
      questions: [
        "Do you use any portfolio analytics or rebalancing software?",
        "What kind of reporting do clients receive—frequency, depth, metrics covered?",
        "Can I access my portfolio in real time through a dashboard or app?",
      ],
      tips: [
        "Example: Research on Morningstar Direct; risk on in‑house Python/risk engine; OMS/rebalance via custodian API; reporting through portal.",
        "Example: Monthly statements with performance, attribution, risk, XIRR, and goal progress. Quarterly deep‑dives include outlook and actions.",
        "Example: Yes—secure web/app with daily holdings, transactions, tax lots, and documents."
      ],
    },
    {
      key: "other",
      title: "Other (Advanced for BL/Views)",
      questions: [
        "How do you generate or source the prior (market equilibrium) returns in your framework?",
        "What is the role of tau (the scalar on the uncertainty of the prior) in your model? How is it chosen?",
        "How do you assign confidence levels to views? Are they subjective or model-driven?",
        "Can you provide a simple example showing how your views changed the portfolio outcome?",
      ],
      tips: [
        "Example: Priors via reverse‑optimising the cap‑weighted market using long‑run vol/corr and equity risk premium; sanity‑checked with historical regimes.",
        "Example: Tau scales prior uncertainty; we set 0.03–0.08 based on historical fit/cross‑validation—smaller tau = stronger prior influence.",
        "Example: Confidence from forecast error stdevs and information coefficients; we cap extreme views and use shrinkage for stability.",
        "Example: 2023: +2% BL view to Indian IT with medium confidence → +120 bps weight shift → +150 bps contribution; later normalised as spread closed."
      ],
    },
    {
      key: "ethics",
      title: "Alignment & Ethics",
      questions: [
        "What are your firm’s guiding values—how do you ensure alignment with the client’s long-term interests?",
        "How do you handle conflicts of interest?",
        "Can you share any case where you advised against investing in a high-fee product or a trend-driven strategy?",
      ],
      tips: [
        "Example: Values—integrity, transparency, stewardship. We align via IPS documentation, fee clarity, and refusing unsuitable products.",
        "Example: We disclose all potential conflicts, avoid revenue‑linked incentives, and document product selection on a best‑interest basis.",
        "Example: Advised against a high‑fee thematic NFO in 2022; recommended diversified, low‑cost exposure instead; outcome was better risk‑adjusted returns."
      ],
    },
  ]), []);

  // --- State ---
  const [active, setActive] = useState(-1); // -1 details, 0..n-1 sections, n review
  const [answers, setAnswers] = useState(() => { try { return JSON.parse(localStorage.getItem("wm_eval_answers")||"{}"); } catch { return {}; } });
  const [meta, setMeta] = useState(() => { try { return JSON.parse(localStorage.getItem("wm_eval_meta")||"{}") || { name:"", email:"", phone:"" }; } catch { return { name:"", email:"", phone:"" }; } });
  const [consent, setConsent] = useState(false);
  const [submitting, setSubmitting] = useState(false);
  const [submitted, setSubmitted] = useState(false);
  const [error, setError] = useState("");
  const [showValidation, setShowValidation] = useState(false);

  useEffect(() => { localStorage.setItem("wm_eval_answers", JSON.stringify(answers)); }, [answers]);
  useEffect(() => { localStorage.setItem("wm_eval_meta", JSON.stringify(meta)); }, [meta]);

  const totalSteps = 1 + sections.length + 1; // details + sections + review
  const stepNumber = active === -1 ? 1 : active + 2;
  const progress = Math.round((stepNumber / totalSteps) * 100);

  const wordCount = (t="") => (t.trim() ? t.trim().split(/\s+/).length : 0);
  const updateAnswer = (sectionKey, qIndex, value) => {
    setAnswers((prev) => ({ ...prev, [sectionKey]: { ...(prev[sectionKey] || {}), [qIndex]: value } }));
  };
  const sectionValid = (idx) => {
    const s = sections[idx]; if (!s) return true; const a = answers[s.key] || {}; return s.questions.every((_, qi) => wordCount(a[qi]||"") >= MIN_WORDS);
  };

  const validateBeforeSubmit = () => {
    if (!meta.name.trim()) return "Please enter your name.";
    if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(meta.email)) return "Please enter a valid email.";
    if (!consent) return "Please accept the privacy notice to continue.";
    if (!FORMSPREE_ENDPOINT || FORMSPREE_ENDPOINT.includes("REPLACE_ME")) return "Submission endpoint is not configured. Please inform the organizer.";
    return "";
  };

  const buildPayload = () => {
    const flat = [];
    sections.forEach((s) => {
      const secAns = answers[s.key] || {};
      s.questions.forEach((q, i) => flat.push({ question: `${s.title} — Q${i+1}`, answer: (secAns[i]||"").trim() }));
    });
    const textSummary = [
      `Client: ${meta.name}`,
      `Email: ${meta.email}`,
      meta.phone ? `Phone: ${meta.phone}` : null,
      "",
      ...flat.map((x) => `${x.question}:\n${x.answer || "(no response)"}\n`),
    ].filter(Boolean).join("\n");
    return { meta, answers, textSummary, submittedAt: new Date().toISOString() };
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setError("");
    const v = validateBeforeSubmit(); if (v) { setError(v); return; }
    setSubmitting(true);
    try {
      const payload = buildPayload();
      const res = await fetch(FORMSPREE_ENDPOINT, { method: "POST", headers: { "Content-Type": "application/json", "Accept": "application/json" }, body: JSON.stringify({ name: payload.meta.name, email: payload.meta.email, phone: payload.meta.phone, summary: payload.textSummary, raw: JSON.stringify(payload), _subject: `Wealth Manager Evaluation — ${payload.meta.name}` }) });
      if (!res.ok) throw new Error(`Failed with status ${res.status}`);
      setSubmitted(true); localStorage.removeItem("wm_eval_answers"); localStorage.removeItem("wm_eval_meta");
    } catch (err) { setError("There was a problem submitting your responses. Please try again."); } finally { setSubmitting(false); }
  };

  const printAsPdf = () => window.print();

  const Review = () => (
    <div className="space-y-6">
      <div className="rounded-2xl border p-6 bg-white shadow-sm">
        <h3 className="text-lg font-semibold mb-4" style={{color: brand.teal}}>Your Details</h3>
        <dl className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div><dt className="text-sm text-gray-500">Name</dt><dd className="font-medium">{meta.name || "—"}</dd></div>
          <div><dt className="text-sm text-gray-500">Email</dt><dd className="font-medium">{meta.email || "—"}</dd></div>
          <div><dt className="text-sm text-gray-500">Phone</dt><dd className="font-medium">{meta.phone || "—"}</dd></div>
        </dl>
      </div>
      {sections.map((s, si) => (
        <div key={s.key} className="rounded-2xl border p-6 bg-white shadow-sm">
          <h3 className="text-lg font-semibold mb-4" style={{color: brand.teal}}>{si + 1}. {s.title}</h3>
          <ol className="space-y-4 list-decimal pl-5">
            {s.questions.map((q, qi) => (
              <li key={qi}>
                <p className="text-sm text-gray-600">{q}</p>
                <p className="mt-1 whitespace-pre-wrap">{(answers[s.key]?.[qi] || "—").trim() || "—"}</p>
              </li>
            ))}
          </ol>
        </div>
      ))}
    </div>
  );

  if (submitted) {
    return (
      <main className="min-h-screen bg-[var(--mist)]" style={{"--mist": brand.mist}}>
        <Header brand={brand} />
        <div className="max-w-4xl mx-auto p-4 md:p-8">
          <div className="rounded-3xl bg-white p-8 shadow-md text-center">
            <h2 className="text-2xl font-bold" style={{color: brand.teal}}>Thank you for your submission</h2>
            <p className="mt-3 text-gray-700">We’ve received your responses. A copy has been sent to the organizer’s inbox.</p>
            <button onClick={() => window.location.reload()} className="mt-6 px-5 py-2 rounded-2xl font-medium" style={{background: brand.green, color: "white"}}>Submit another response</button>
          </div>
        </div>
      </main>
    );
  }

  const current = sections[active] || null;

  return (
    <main className="min-h-screen bg-[var(--mist)]" style={{"--mist": brand.mist}}>
      <Header brand={brand} />

      <div className="max-w-5xl mx-auto p-4 md:p-8">
        <div className="rounded-3xl bg-white p-6 md:p-8 shadow-md">
          {/* Progress */}
          <div className="mb-6">
            <div className="flex items-center justify-between mb-2">
              <span className="text-sm font-medium" style={{color: brand.teal}}>Progress</span>
              <span className="text-sm text-gray-600">{stepNumber} / {totalSteps}</span>
            </div>
            <div className="h-2 w-full bg-gray-200 rounded-full overflow-hidden">
              <div className="h-full transition-all" style={{ width: `${progress}%`, background: brand.green }} />
            </div>
          </div>

          {/* DETAILS PAGE FIRST */}
          {active === -1 && (
            <div>
              <h2 className="text-xl md:text-2xl font-semibold mb-4" style={{color: brand.teal}}>Your Details</h2>
              <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                <div>
                  <label className="text-sm text-gray-700">Your Name *</label>
                  <input value={meta.name} onChange={(e) => setMeta((m) => ({ ...m, name: e.target.value }))} className="mt-1 w-full rounded-xl border px-3 py-2 focus:outline-none" placeholder="Jane Doe" />
                </div>
                <div>
                  <label className="text-sm text-gray-700">Email *</label>
                  <input type="email" value={meta.email} onChange={(e) => setMeta((m) => ({ ...m, email: e.target.value }))} className="mt-1 w-full rounded-xl border px-3 py-2 focus:outline-none" placeholder="jane@example.com" />
                </div>
                <div>
                  <label className="text-sm text-gray-700">Phone</label>
                  <input value={meta.phone} onChange={(e) => setMeta((m) => ({ ...m, phone: e.target.value }))} className="mt-1 w-full rounded-xl border px-3 py-2 focus:outline-none" placeholder="Optional" />
                </div>
              </div>
              <div className="flex items-center gap-2 mb-6">
                <input id="consent" type="checkbox" checked={consent} onChange={(e) => setConsent(e.target.checked)} />
                <label htmlFor="consent" className="text-sm text-gray-700">I agree to share these responses for evaluation purposes and acknowledge the privacy notice below.</label>
              </div>
              <div className="flex justify-end">
                <button
                  onClick={() => {
                    if (!meta.name.trim() || !/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(meta.email)) {
                      setError("Please enter your name and a valid email before starting.");
                      return;
                    }
                    setError("");
                    setActive(0);
                  }}
                  className="px-5 py-2 rounded-2xl font-semibold"
                  style={{ background: brand.green, color: "white" }}
                >
                  Start Questionnaire
                </button>
              </div>
              {error && <div className="mt-4 p-3 rounded-xl bg-red-50 border border-red-200 text-red-800 text-sm">{error}</div>}
            </div>
          )}

          {/* QUESTION PAGES */}
          {active >= 0 && active < sections.length && current && (
            <div>
              <h2 className="text-xl md:text-2xl font-semibold mb-4" style={{color: brand.teal}}>
                {active + 1}. {current.title}
              </h2>
              <ol className="space-y-5 list-decimal pl-5">
                {current.questions.map((q, i) => (
                  <li key={i}>
                    <p className="text-sm text-gray-700 mb-2">{q}</p>
                    <textarea
                      className="w-full min-h-[120px] rounded-xl border px-3 py-2 focus:outline-none"
                      placeholder="Write at least 3–4 sentences…"
                      value={(answers[current.key]?.[i]) || ""}
                      onChange={(e) => updateAnswer(current.key, i, e.target.value)}
                    />
                    {/* Helper: word count + guidance */}
                    <div className="mt-2 text-xs">
                      <span className="text-gray-500">Words: {wordCount((answers[current.key]?.[i]) || "")} / {MIN_WORDS}</span>
                      {showValidation && wordCount((answers[current.key]?.[i]) || "") < MIN_WORDS && (
                        <div className="mt-1 p-2 rounded-lg border border-red-200 bg-red-50 text-red-700">Please write at least {MIN_WORDS} words.</div>
                      )}
                      <details className="mt-2">
                        <summary className="cursor-pointer text-gray-600">Need help? See a demo answer</summary>
                        <div className="mt-1 text-gray-600">{(current.tips && current.tips[i]) || "Aim for a clear answer covering the what, why, and how."}</div>
                      </details>
                    </div>
                  </li>
                ))}
              </ol>
            </div>
          )}

          {/* REVIEW PAGE */}
          {active === sections.length && (
            <div>
              <h2 className="text-xl md:text-2xl font-semibold mb-4" style={{color: brand.teal}}>Review your responses</h2>
              <Review />
            </div>
          )}

          {/* Footer controls */}
          <div className="mt-8 flex flex-col md:flex-row md:items-center gap-3 md:justify-between">
            <div />
            <div className="flex gap-2">
              <button
                onClick={() => setActive((a) => (a === -1 ? -1 : Math.max(-1, a - 1)))}
                disabled={active === -1}
                className="px-4 py-2 rounded-2xl border font-medium disabled:opacity-50"
                style={{ borderColor: brand.teal, color: brand.teal }}
                type="button"
              >
                Back
              </button>
              {active < sections.length && (
                <button
                  onClick={() => {
                    if (active >= 0 && active < sections.length && !sectionValid(active)) { setShowValidation(true); return; }
                    setShowValidation(false);
                    setActive((a) => (a === -1 ? 0 : Math.min(sections.length, a + 1)));
                  }}
                  className="px-4 py-2 rounded-2xl font-medium"
                  style={{ background: brand.green, color: "white" }}
                  type="button"
                >
                  Next
                </button>
              )}
              {active === sections.length && (
                <button onClick={handleSubmit} disabled={submitting} className="px-4 py-2 rounded-2xl font-semibold flex items-center justify-center gap-2 disabled:opacity-60" style={{ background: brand.teal, color: "white" }}>
                  {submitting ? "Submitting…" : "Submit"}
                </button>
              )}
              <button onClick={printAsPdf} type="button" className="px-4 py-2 rounded-2xl border font-medium" style={{ borderColor: brand.gold, color: brand.teal }}>
                Print / Save PDF
              </button>
            </div>
          </div>

          {error && active !== -1 && (
            <div className="mt-4 p-3 rounded-xl bg-red-50 border border-red-200 text-red-800 text-sm">{error}</div>
          )}

          {/* Privacy */}
          <div className="mt-8 text-xs text-gray-600 leading-relaxed">
            <p className="font-semibold" style={{color: brand.teal}}>Privacy Notice</p>
            <p>
              Your responses are used solely for evaluating wealth management alignment and will not be shared outside Growthfiniti without your permission. Do not include account numbers, passwords, or confidential credentials. By submitting, you consent to be contacted for clarification if needed.
            </p>
          </div>
        </div>

        <div className="text-center text-xs text-gray-500 mt-6">
          <p>© {new Date().getFullYear()} Growthfiniti Wealth Pvt Ltd · SEBI PMS: INP000009418 · AMFI ARN-168766</p>
        </div>
      </div>

      <style>{`
        @media print { header, .no-print { display: none !important; } main { background: white !important; } textarea { border: 1px solid #ddd !important; } }
      `}</style>
    </main>
  );
}

function Header({ brand }) {
  return (
    <header className="no-print border-b" style={{ borderColor: "#E6EEEC" }}>
      <div className="max-w-5xl mx-auto px-4 md:px-8 py-6 flex items-center justify-between">
        <div className="flex items-center gap-3">
          <img src="/growthfiniti-logo-symbol.png" alt="Growthfiniti Logo" className="w-10 h-10 rounded-2xl" />
          <div>
            <h1 className="text-xl md:text-2xl font-bold" style={{ color: brand.teal }}>Wealth Manager Evaluation</h1>
            <p className="text-xs text-gray-600">Answer thoughtfully • Save & resume automatically</p>
          </div>
        </div>
        <a href="#" className="text-sm font-medium px-4 py-2 rounded-2xl" style={{ background: brand.gold, color: brand.teal }}>Growthfiniti</a>
      </div>
    </header>
  );
}
