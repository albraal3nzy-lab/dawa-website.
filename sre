import { useState, useMemo } from "react";
import { Search, MapPin, Plus, Trash2, LogOut, Check, X, Store, User, ShieldCheck, Building2, CreditCard, Globe, UserPlus, MessageSquare, ArrowRight, ArrowLeft } from "lucide-react";

const FONT_IMPORT = `@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800&family=Tajawal:wght@400;500;700&family=Inter:wght@400;600;700;800&display=swap');`;

const COLORS = {
  deep: "#123832", deepAlt: "#1B4B43", sand: "#F6F1E6", sage: "#E7EEE7",
  amber: "#D98E3F", charcoal: "#26241F", red: "#B24435", line: "#D8CDB8",
};

// ---------------- i18n ----------------
const STR = {
  ar: {
    brand: "دواء", tagline: "شبكة توفر الأدوية في الصيدليات",
    patient: "مريض", pharmacy: "صيدلية",
    heroPatient: "دوّر على دوائك، وشوف وين لاقيه",
    heroCompany: "شركتك، كل فروعها بحساب واحد",
    subPatient: "اكتب اسم الدواء وشوف فوراً أي الصيدليات القريبة عندها بالمخزون.",
    subCompany: "سجّل دخول شركتك وأدر مخزون كل فرع من فروعك بشكل منفصل.",
    medicineName: "اسم الدواء", searchPh: "مثال: بنادول، أوغمنتين...",
    noResults: "ما في صيدلية مسجلة عندها هذا الدواء حالياً.",
    rxTitle: "وصفتي — التحقق من Medic", rxSub: "وصفاتك المرتبطة برقم هاتفك تلقائياً.",
    rxNone: "ما في وصفة مسجلة على رقمك حالياً.",
    prescribedBy: "بوصفة من:",
    loginCompany: "دخول الشركة", loginPatient: "دخول المريض",
    signupPatient: "حساب جديد",
    phone: "رقم الهاتف", password: "كلمة المرور",
    firstName: "الاسم الأول", lastName: "اسم العائلة", email: "البريد الإلكتروني", nationalId: "الرقم الوطني / رقم الهوية",
    login: "دخول", createAccount: "إنشاء الحساب",
    haveAccount: "عندك حساب؟ دخول", noAccount: "ما عندك حساب؟ سجّل",
    tryCompany: "جرّب: 0933 111 222 / 1234",
    tryPatientOtp: "جرّب رقم: 0999115115",
    wrongCreds: "رقم الهاتف أو كلمة المرور غير صحيحة",
    fillAll: "عبّي كل الحقول",
    logout: "خروج",
    subscription: "الاشتراك",
    branches: "الفروع", newBranchPh: "منطقة الفرع الجديد", addBranch: "فرع جديد",
    branchInv: "أدوية فرع:", noMeds: "ما ضفت أدوية بعد لهذا الفرع.",
    newMedPh: "اسم الدواء الجديد", price: "السعر", add: "إضافة",
    inStock: "متوفر", outStock: "غير متوفر",
    welcome: "أهلاً",
    sendCode: "إرسال رمز التحقق", enterCode: "أدخل رمز التحقق",
    codeSentTo: "أرسلنا رمز مؤلف من 4 أرقام إلى", verifyCode: "تأكيد",
    resend: "إعادة إرسال الرمز", changeNumber: "تغيير الرقم",
    demoNote: "هذا نموذج تجريبي — رمز التحقق:",
    wrongCode: "الرمز غير صحيح",
    noAccountFound: "ما في حساب مسجل بهذا الرقم — سجّل حساب جديد أول.",
    invalidPhone: "أدخل رقم هاتف صحيح",
  },
  en: {
    brand: "Dawa", tagline: "Pharmacy medicine availability network",
    patient: "Patient", pharmacy: "Pharmacy",
    heroPatient: "Find your medicine, and where it's in stock",
    heroCompany: "Your company, all branches, one account",
    subPatient: "Type a medicine name and instantly see which nearby pharmacies have it.",
    subCompany: "Log in to your company account and manage each branch's stock separately.",
    medicineName: "Medicine name", searchPh: "e.g. Panadol, Augmentin...",
    noResults: "No registered pharmacy currently has this medicine.",
    rxTitle: "My Prescriptions — Medic verification", rxSub: "Prescriptions linked to your phone number automatically.",
    rxNone: "No prescriptions found for your number.",
    prescribedBy: "Prescribed by:",
    loginCompany: "Company Login", loginPatient: "Patient Login",
    signupPatient: "Create account",
    phone: "Phone number", password: "Password",
    firstName: "First name", lastName: "Last name", email: "Email", nationalId: "National / ID number",
    login: "Log in", createAccount: "Create account",
    haveAccount: "Have an account? Log in", noAccount: "No account? Sign up",
    tryCompany: "Try: 0933 111 222 / 1234",
    tryPatientOtp: "Try number: 0999115115",
    wrongCreds: "Incorrect phone number or password",
    fillAll: "Please fill all fields",
    logout: "Log out",
    subscription: "Subscription",
    branches: "Branches", newBranchPh: "New branch area", addBranch: "Add branch",
    branchInv: "Medicines at:", noMeds: "No medicines added to this branch yet.",
    newMedPh: "New medicine name", price: "Price", add: "Add",
    inStock: "In stock", outStock: "Out of stock",
    welcome: "Welcome",
    sendCode: "Send verification code", enterCode: "Enter verification code",
    codeSentTo: "We sent a 4-digit code to", verifyCode: "Verify",
    resend: "Resend code", changeNumber: "Change number",
    demoNote: "Demo mode — verification code:",
    wrongCode: "Incorrect code",
    noAccountFound: "No account found with this number — please sign up first.",
    invalidPhone: "Enter a valid phone number",
  },
};

// ---------------- seed data ----------------
const seedCompanies = [
  { id: "c1", name: "شركة الدواء", nameEn: "Dawa Co.", phone: "0933 111 222", password: "1234", plan: "monthly_5usd",
    branches: [
      { id: "b1", area: "دمشق - المزة", areaEn: "Damascus - Mazzeh", inventory: [
        { id: "m1", name: "بنادول", nameEn: "Panadol", price: 3000, stock: "متوفر" },
        { id: "m2", name: "أوغمنتين", nameEn: "Augmentin", price: 12000, stock: "متوفر" },
      ]},
      { id: "b2", area: "دمشق - المالكي", areaEn: "Damascus - Malki", inventory: [
        { id: "m3", name: "فيتامين د", nameEn: "Vitamin D", price: 8000, stock: "غير متوفر" },
      ]},
    ]},
  { id: "c2", name: "صيدليات الشفاء", nameEn: "Al-Shifa Pharmacies", phone: "0944 222 333", password: "1234", plan: "monthly_5usd",
    branches: [
      { id: "b3", area: "حلب - الفرقان", areaEn: "Aleppo - Furqan", inventory: [
        { id: "m5", name: "بنادول", nameEn: "Panadol", price: 2800, stock: "متوفر" },
      ]},
    ]},
];

const seedPatients = [
  { id: "p1", phone: "0999115115", firstName: "أحمد", lastName: "قاسم", email: "ahmad@example.com", nationalId: "01234567890", password: "1234" },
];

const mockPrescriptions = {
  "0999115115": [{ drug: "أوغمنتين", drugEn: "Augmentin", dose: "1 قرص كل 12 ساعة - 7 أيام", doseEn: "1 tablet every 12h - 7 days", doctor: "د. سامر خطيب", doctorEn: "Dr. Samer Khatib" }],
};

function StockBadge({ stock, t }) {
  const ok = stock === "متوفر";
  return (
    <span style={{ display: "inline-flex", alignItems: "center", gap: 6, fontSize: 13, fontWeight: 700, padding: "4px 10px", borderRadius: 999, color: ok ? COLORS.deep : COLORS.red, background: ok ? "#DCEBE1" : "#F3DEDA", border: `1px solid ${ok ? "#B7D6C4" : "#E3BDB4"}` }}>
      {ok ? <Check size={13} /> : <X size={13} />}
      {ok ? t.inStock : t.outStock}
    </span>
  );
}

function Stamp() {
  return (
    <div style={{ position: "absolute", top: -14, insetInlineStart: -10, width: 56, height: 56, transform: "rotate(-14deg)", opacity: 0.9, pointerEvents: "none" }} aria-hidden="true">
      <svg viewBox="0 0 56 56" width="56" height="56">
        <circle cx="28" cy="28" r="26" fill="none" stroke={COLORS.amber} strokeWidth="2" strokeDasharray="3 3" />
        <rect x="23" y="14" width="10" height="28" rx="2" fill={COLORS.amber} />
        <rect x="14" y="23" width="28" height="10" rx="2" fill={COLORS.amber} />
      </svg>
    </div>
  );
}

function Card({ children, style }) {
  return <div style={{ background: "#fff", borderRadius: 18, border: `1px solid ${COLORS.line}`, boxShadow: "0 10px 30px rgba(18,56,50,0.08)", padding: 22, position: "relative", ...style }}>{children}</div>;
}

const inputStyle = { padding: "11px 12px", borderRadius: 10, border: `1px solid ${COLORS.line}`, fontSize: 14, fontFamily: "inherit", width: "100%" };

function genOtp() {
  return String(Math.floor(1000 + Math.random() * 9000));
}

export default function App() {
  const [lang, setLang] = useState("ar");
  const t = STR[lang];
  const dir = lang === "ar" ? "rtl" : "ltr";
  const nameKey = lang === "ar" ? "name" : "nameEn";
  const areaKey = lang === "ar" ? "area" : "areaEn";
  const medNameKey = lang === "ar" ? "name" : "nameEn";
  const ArrowIcon = lang === "ar" ? ArrowLeft : ArrowRight;

  const [mode, setMode] = useState("patient");
  const [companies, setCompanies] = useState(seedCompanies);
  const [patients, setPatients] = useState(seedPatients);
  const [query, setQuery] = useState("");

  // company auth
  const [companyPhone, setCompanyPhone] = useState("");
  const [companyPass, setCompanyPass] = useState("");
  const [companyErr, setCompanyErr] = useState("");
  const [activeCompanyId, setActiveCompanyId] = useState(null);
  const [activeBranchId, setActiveBranchId] = useState(null);

  // patient signup fields
  const [patientAuthView, setPatientAuthView] = useState("login"); // login | signup
  const [firstName, setFirstName] = useState("");
  const [lastName, setLastName] = useState("");
  const [signupPhone, setSignupPhone] = useState("");
  const [email, setEmail] = useState("");
  const [nationalId, setNationalId] = useState("");
  const [signupPass, setSignupPass] = useState("");
  const [signupErr, setSignupErr] = useState("");

  // patient OTP login
  const [otpStep, setOtpStep] = useState("phone"); // phone | code
  const [otpPhone, setOtpPhone] = useState("");
  const [otpGenerated, setOtpGenerated] = useState("");
  const [otpInput, setOtpInput] = useState("");
  const [otpErr, setOtpErr] = useState("");
  const [activePatientId, setActivePatientId] = useState(null);

  const [newName, setNewName] = useState("");
  const [newPrice, setNewPrice] = useState("");
  const [newBranchArea, setNewBranchArea] = useState("");

  const activeCompany = companies.find((c) => c.id === activeCompanyId);
  const activeBranch = activeCompany?.branches.find((b) => b.id === activeBranchId);
  const activePatient = patients.find((p) => p.id === activePatientId);

  const results = useMemo(() => {
    if (!query.trim()) return [];
    const q = query.trim().toLowerCase();
    const out = [];
    companies.forEach((co) => co.branches.forEach((br) => {
      const match = br.inventory.find((m) => (m.name + " " + (m.nameEn || "")).toLowerCase().includes(q));
      if (match) out.push({ co, br, match });
    }));
    return out;
  }, [query, companies]);

  const rxResult = activePatient ? mockPrescriptions[activePatient.phone.replace(/\s/g, "")] || [] : null;

  function handleCompanyLogin(e) {
    e.preventDefault();
    const found = companies.find((c) => c.phone.replace(/\s/g, "") === companyPhone.replace(/\s/g, ""));
    if (!found || found.password !== companyPass) { setCompanyErr(t.wrongCreds); return; }
    setCompanyErr(""); setActiveCompanyId(found.id); setActiveBranchId(found.branches[0]?.id ?? null);
  }
  function handleCompanyLogout() { setActiveCompanyId(null); setActiveBranchId(null); setCompanyPhone(""); setCompanyPass(""); }

  function handleSignup(e) {
    e.preventDefault();
    if (!firstName.trim() || !lastName.trim() || !signupPhone.trim() || !email.trim() || !nationalId.trim() || !signupPass.trim()) {
      setSignupErr(t.fillAll); return;
    }
    const id = "p" + Date.now();
    const newP = { id, firstName: firstName.trim(), lastName: lastName.trim(), phone: signupPhone.trim(), email: email.trim(), nationalId: nationalId.trim(), password: signupPass };
    setPatients((prev) => [...prev, newP]);
    setSignupErr("");
    setActivePatientId(id);
  }

  function sendOtp(e) {
    e.preventDefault();
    if (!otpPhone.trim() || otpPhone.trim().length < 6) { setOtpErr(t.invalidPhone); return; }
    setOtpErr("");
    setOtpGenerated(genOtp());
    setOtpStep("code");
  }
  function verifyOtp(e) {
    e.preventDefault();
    if (otpInput.trim() !== otpGenerated) { setOtpErr(t.wrongCode); return; }
    const found = patients.find((p) => p.phone.replace(/\s/g, "") === otpPhone.replace(/\s/g, ""));
    if (!found) { setOtpErr(t.noAccountFound); return; }
    setOtpErr("");
    setActivePatientId(found.id);
  }
  function resetOtp() { setOtpStep("phone"); setOtpInput(""); setOtpErr(""); setOtpGenerated(""); }
  function handlePatientLogout() {
    setActivePatientId(null); resetOtp(); setOtpPhone("");
    setFirstName(""); setLastName(""); setSignupPhone(""); setEmail(""); setNationalId(""); setSignupPass("");
    setPatientAuthView("login");
  }

  function toggleStock(medId) {
    setCompanies((prev) => prev.map((c) => c.id !== activeCompanyId ? c : { ...c, branches: c.branches.map((b) => b.id !== activeBranchId ? b : { ...b, inventory: b.inventory.map((m) => m.id === medId ? { ...m, stock: m.stock === "متوفر" ? "غير متوفر" : "متوفر" } : m) }) }));
  }
  function removeMed(medId) {
    setCompanies((prev) => prev.map((c) => c.id !== activeCompanyId ? c : { ...c, branches: c.branches.map((b) => b.id !== activeBranchId ? b : { ...b, inventory: b.inventory.filter((m) => m.id !== medId) }) }));
  }
  function addMed(e) {
    e.preventDefault();
    if (!newName.trim()) return;
    setCompanies((prev) => prev.map((c) => c.id !== activeCompanyId ? c : { ...c, branches: c.branches.map((b) => b.id !== activeBranchId ? b : { ...b, inventory: [...b.inventory, { id: "m" + Date.now(), name: newName.trim(), nameEn: newName.trim(), price: Number(newPrice) || 0, stock: "متوفر" }] }) }));
    setNewName(""); setNewPrice("");
  }
  function addBranch(e) {
    e.preventDefault();
    if (!newBranchArea.trim()) return;
    const id = "b" + Date.now();
    setCompanies((prev) => prev.map((c) => c.id !== activeCompanyId ? c : { ...c, branches: [...c.branches, { id, area: newBranchArea.trim(), areaEn: newBranchArea.trim(), inventory: [] }] }));
    setNewBranchArea(""); setActiveBranchId(id);
  }

  return (
    <div dir={dir} style={{ fontFamily: lang === "ar" ? "'Tajawal', 'Cairo', sans-serif" : "'Inter', sans-serif", background: COLORS.sand, minHeight: "100vh", color: COLORS.charcoal, padding: "0 0 60px" }}>
      <style>{`
        ${FONT_IMPORT}
        * { box-sizing: border-box; }
        input:focus, button:focus { outline: 2px solid ${COLORS.amber}; outline-offset: 2px; }
        button { cursor: pointer; font-family: inherit; }
        @media (prefers-reduced-motion: reduce) { * { transition: none !important; } }
      `}</style>

      <header style={{ background: COLORS.deep, color: COLORS.sand, padding: "22px 20px 60px", position: "relative", overflow: "hidden" }}>
        <div style={{ maxWidth: 720, margin: "0 auto", position: "relative" }}>
          <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", flexWrap: "wrap", gap: 10 }}>
            <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
              <div style={{ width: 38, height: 38, borderRadius: 10, background: COLORS.amber, display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 800, fontFamily: lang === "ar" ? "'Cairo', sans-serif" : "'Inter', sans-serif", fontSize: 18, color: COLORS.deep }}>{lang === "ar" ? "د" : "D"}</div>
              <div>
                <div style={{ fontFamily: lang === "ar" ? "'Cairo', sans-serif" : "'Inter', sans-serif", fontWeight: 800, fontSize: 20 }}>{t.brand}</div>
                <div style={{ fontSize: 12, opacity: 0.75 }}>{t.tagline}</div>
              </div>
            </div>
            <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
              <button onClick={() => setLang(lang === "ar" ? "en" : "ar")} style={{ display: "flex", alignItems: "center", gap: 6, background: "rgba(255,255,255,0.1)", border: "1px solid rgba(255,255,255,0.2)", borderRadius: 999, padding: "8px 12px", color: COLORS.sand, fontSize: 13, fontWeight: 700 }}>
                <Globe size={14} /> {lang === "ar" ? "EN" : "AR"}
              </button>
              <div style={{ display: "flex", background: "rgba(255,255,255,0.08)", borderRadius: 999, padding: 4, gap: 4 }}>
                <button onClick={() => setMode("patient")} style={{ display: "flex", alignItems: "center", gap: 6, padding: "8px 14px", borderRadius: 999, border: "none", fontWeight: 700, fontSize: 13, background: mode === "patient" ? COLORS.amber : "transparent", color: mode === "patient" ? COLORS.deep : COLORS.sand }}>
                  <User size={15} /> {t.patient}
                </button>
                <button onClick={() => setMode("pharmacy")} style={{ display: "flex", alignItems: "center", gap: 6, padding: "8px 14px", borderRadius: 999, border: "none", fontWeight: 700, fontSize: 13, background: mode === "pharmacy" ? COLORS.amber : "transparent", color: mode === "pharmacy" ? COLORS.deep : COLORS.sand }}>
                  <Store size={15} /> {t.pharmacy}
                </button>
              </div>
            </div>
          </div>
          <h1 style={{ fontFamily: lang === "ar" ? "'Cairo', sans-serif" : "'Inter', sans-serif", fontWeight: 800, fontSize: 26, marginTop: 34, marginBottom: 6, maxWidth: 460, lineHeight: 1.5 }}>{mode === "patient" ? t.heroPatient : t.heroCompany}</h1>
          <p style={{ fontSize: 14, opacity: 0.8, maxWidth: 440 }}>{mode === "patient" ? t.subPatient : t.subCompany}</p>
        </div>
      </header>

      <main style={{ maxWidth: 720, margin: "-36px auto 0", padding: "0 20px", position: "relative" }}>
        {mode === "patient" ? (
          activePatient ? (
            <PatientView t={t} lang={lang} query={query} setQuery={setQuery} results={results} areaKey={areaKey} rxResult={rxResult} activePatient={activePatient} onLogout={handlePatientLogout} />
          ) : patientAuthView === "login" ? (
            <OtpLoginCard t={t} ArrowIcon={ArrowIcon} step={otpStep} phone={otpPhone} setPhone={setOtpPhone} otpInput={otpInput} setOtpInput={setOtpInput} otpGenerated={otpGenerated} err={otpErr} onSend={sendOtp} onVerify={verifyOtp} onReset={resetOtp} setView={setPatientAuthView} />
          ) : (
            <SignupCard t={t} firstName={firstName} setFirstName={setFirstName} lastName={lastName} setLastName={setLastName} phone={signupPhone} setPhone={setSignupPhone} email={email} setEmail={setEmail} nationalId={nationalId} setNationalId={setNationalId} pass={signupPass} setPass={setSignupPass} err={signupErr} onSubmit={handleSignup} setView={setPatientAuthView} />
          )
        ) : activeCompany ? (
          <CompanyDashboard t={t} lang={lang} company={activeCompany} nameKey={nameKey} areaKey={areaKey} medNameKey={medNameKey} activeBranch={activeBranch} setActiveBranchId={setActiveBranchId} onLogout={handleCompanyLogout} toggleStock={toggleStock} removeMed={removeMed} addMed={addMed} newName={newName} setNewName={setNewName} newPrice={newPrice} setNewPrice={setNewPrice} addBranch={addBranch} newBranchArea={newBranchArea} setNewBranchArea={setNewBranchArea} />
        ) : (
          <CompanyLoginCard t={t} phone={companyPhone} setPhone={setCompanyPhone} pass={companyPass} setPass={setCompanyPass} err={companyErr} onLogin={handleCompanyLogin} />
        )}
      </main>
    </div>
  );
}

function OtpLoginCard({ t, ArrowIcon, step, phone, setPhone, otpInput, setOtpInput, otpGenerated, err, onSend, onVerify, onReset, setView }) {
  return (
    <Card style={{ maxWidth: 420, margin: "0 auto" }}>
      <Stamp />
      <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 4 }}>
        <MessageSquare size={18} color={COLORS.deepAlt} />
        <div style={{ fontFamily: "'Cairo','Inter', sans-serif", fontWeight: 800, fontSize: 18 }}>{t.loginPatient}</div>
      </div>
      <p style={{ fontSize: 13, color: "#8a8677", marginBottom: 16 }}>{t.tryPatientOtp}</p>

      {step === "phone" ? (
        <form onSubmit={onSend} style={{ display: "flex", flexDirection: "column", gap: 10 }}>
          <input value={phone} onChange={(e) => setPhone(e.target.value)} placeholder={t.phone} style={inputStyle} />
          {err && <div style={{ color: COLORS.red, fontSize: 13 }}>{err}</div>}
          <button type="submit" style={{ display: "flex", alignItems: "center", justifyContent: "center", gap: 6, background: COLORS.amber, color: COLORS.deep, border: "none", borderRadius: 10, padding: "12px", fontWeight: 800, fontSize: 15, marginTop: 4 }}>
            {t.sendCode} <ArrowIcon size={16} />
          </button>
        </form>
      ) : (
        <form onSubmit={onVerify} style={{ display: "flex", flexDirection: "column", gap: 10 }}>
          <p style={{ fontSize: 13, color: COLORS.charcoal }}>{t.codeSentTo} <b>{phone}</b></p>
          <div style={{ background: COLORS.sage, border: `1px dashed ${COLORS.line}`, borderRadius: 10, padding: "8px 12px", fontSize: 13, color: COLORS.deepAlt }}>
            {t.demoNote} <b style={{ fontSize: 16, letterSpacing: 2 }}>{otpGenerated}</b>
          </div>
          <input value={otpInput} onChange={(e) => setOtpInput(e.target.value)} placeholder="0000" maxLength={4} style={{ ...inputStyle, textAlign: "center", fontSize: 22, letterSpacing: 8, fontWeight: 800 }} />
          {err && <div style={{ color: COLORS.red, fontSize: 13 }}>{err}</div>}
          <button type="submit" style={{ background: COLORS.amber, color: COLORS.deep, border: "none", borderRadius: 10, padding: "12px", fontWeight: 800, fontSize: 15, marginTop: 4 }}>{t.verifyCode}</button>
          <button type="button" onClick={onReset} style={{ background: "transparent", border: "none", color: COLORS.deepAlt, fontSize: 13, fontWeight: 700 }}>{t.changeNumber}</button>
        </form>
      )}

      <button onClick={() => setView("signup")} style={{ background: "transparent", border: "none", color: COLORS.deepAlt, fontSize: 13, fontWeight: 700, marginTop: 12, display: "flex", alignItems: "center", gap: 5 }}>
        <UserPlus size={14} /> {t.noAccount}
      </button>
    </Card>
  );
}

function SignupCard({ t, firstName, setFirstName, lastName, setLastName, phone, setPhone, email, setEmail, nationalId, setNationalId, pass, setPass, err, onSubmit, setView }) {
  return (
    <Card style={{ maxWidth: 420, margin: "0 auto" }}>
      <Stamp />
      <div style={{ fontFamily: "'Cairo','Inter', sans-serif", fontWeight: 800, fontSize: 18, marginBottom: 16 }}>{t.signupPatient}</div>
      <form onSubmit={onSubmit} style={{ display: "flex", flexDirection: "column", gap: 10 }}>
        <div style={{ display: "flex", gap: 8 }}>
          <input value={firstName} onChange={(e) => setFirstName(e.target.value)} placeholder={t.firstName} style={inputStyle} />
          <input value={lastName} onChange={(e) => setLastName(e.target.value)} placeholder={t.lastName} style={inputStyle} />
        </div>
        <input value={phone} onChange={(e) => setPhone(e.target.value)} placeholder={t.phone} style={inputStyle} />
        <input value={email} onChange={(e) => setEmail(e.target.value)} placeholder={t.email} type="email" style={inputStyle} />
        <input value={nationalId} onChange={(e) => setNationalId(e.target.value)} placeholder={t.nationalId} style={inputStyle} />
        <input value={pass} onChange={(e) => setPass(e.target.value)} placeholder={t.password} type="password" style={inputStyle} />
        {err && <div style={{ color: COLORS.red, fontSize: 13 }}>{err}</div>}
        <button type="submit" style={{ background: COLORS.amber, color: COLORS.deep, border: "none", borderRadius: 10, padding: "12px", fontWeight: 800, fontSize: 15, marginTop: 4 }}>{t.createAccount}</button>
      </form>
      <button onClick={() => setView("login")} style={{ background: "transparent", border: "none", color: COLORS.deepAlt, fontSize: 13, fontWeight: 700, marginTop: 12 }}>{t.haveAccount}</button>
    </Card>
  );
}

function CompanyLoginCard({ t, phone, setPhone, pass, setPass, err, onLogin }) {
  return (
    <Card style={{ maxWidth: 420, margin: "0 auto" }}>
      <Stamp />
      <div style={{ fontFamily: "'Cairo','Inter', sans-serif", fontWeight: 800, fontSize: 18, marginBottom: 4 }}>{t.loginCompany}</div>
      <p style={{ fontSize: 13, color: "#8a8677", marginBottom: 16 }}>{t.tryCompany}</p>
      <form onSubmit={onLogin} style={{ display: "flex", flexDirection: "column", gap: 10 }}>
        <input value={phone} onChange={(e) => setPhone(e.target.value)} placeholder={t.phone} style={inputStyle} />
        <input value={pass} onChange={(e) => setPass(e.target.value)} placeholder={t.password} type="password" style={inputStyle} />
        {err && <div style={{ color: COLORS.red, fontSize: 13 }}>{err}</div>}
        <button type="submit" style={{ background: COLORS.amber, color: COLORS.deep, border: "none", borderRadius: 10, padding: "12px", fontWeight: 800, fontSize: 15, marginTop: 4 }}>{t.login}</button>
      </form>
    </Card>
  );
}

function PatientView({ t, lang, query, setQuery, results, areaKey, rxResult, activePatient, onLogout }) {
  return (
    <div style={{ display: "flex", flexDirection: "column", gap: 18 }}>
      <Card style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
        <div style={{ fontWeight: 700, fontSize: 14 }}>{t.welcome}, {activePatient.firstName}</div>
        <button onClick={onLogout} style={{ display: "flex", alignItems: "center", gap: 6, background: COLORS.sage, border: `1px solid ${COLORS.line}`, borderRadius: 10, padding: "8px 14px", fontSize: 13, fontWeight: 700, color: COLORS.deepAlt }}>
          <LogOut size={14} /> {t.logout}
        </button>
      </Card>

      <Card>
        <Stamp />
        <label style={{ fontSize: 13, fontWeight: 700, color: COLORS.deepAlt, display: "block", marginBottom: 8 }}>{t.medicineName}</label>
        <div style={{ position: "relative" }}>
          <Search size={18} style={{ position: "absolute", insetInlineStart: 14, top: 14, color: "#8a8677" }} />
          <input value={query} onChange={(e) => setQuery(e.target.value)} placeholder={t.searchPh} style={{ ...inputStyle, padding: lang === "ar" ? "12px 44px 12px 14px" : "12px 14px 12px 44px" }} />
        </div>
        {query.trim() && (
          <div style={{ marginTop: 18, display: "flex", flexDirection: "column", gap: 10 }}>
            {results.length === 0 && <div style={{ fontSize: 14, color: "#8a8677", padding: "10px 4px" }}>{t.noResults}</div>}
            {results.map(({ co, br, match }, i) => (
              <div key={i} style={{ border: `1px solid ${COLORS.line}`, borderRadius: 14, padding: 14, display: "flex", justifyContent: "space-between", alignItems: "center", gap: 10 }}>
                <div>
                  <div style={{ fontWeight: 700, fontSize: 15 }}>{lang === "ar" ? co.name : co.nameEn}</div>
                  <div style={{ fontSize: 13, color: "#8a8677", display: "flex", alignItems: "center", gap: 5, marginTop: 3 }}><MapPin size={13} /> {br[areaKey]}</div>
                </div>
                <div style={{ textAlign: lang === "ar" ? "left" : "right" }}>
                  <StockBadge stock={match.stock} t={t} />
                  <div style={{ fontSize: 13, marginTop: 8, fontWeight: 700 }}>{match.price.toLocaleString()} {lang === "ar" ? "ل.س" : "SYP"}</div>
                </div>
              </div>
            ))}
          </div>
        )}
      </Card>

      <Card>
        <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 4 }}>
          <ShieldCheck size={18} color={COLORS.deepAlt} />
          <div style={{ fontWeight: 700, fontSize: 15 }}>{t.rxTitle}</div>
        </div>
        <p style={{ fontSize: 13, color: "#8a8677", marginTop: 4, marginBottom: 14 }}>{t.rxSub}</p>
        {rxResult && rxResult.length > 0 ? (
          rxResult.map((r, i) => (
            <div key={i} style={{ border: `1px solid ${COLORS.line}`, borderRadius: 12, padding: 12, marginBottom: 8 }}>
              <div style={{ fontWeight: 700 }}>{lang === "ar" ? r.drug : r.drugEn}</div>
              <div style={{ fontSize: 13, color: "#8a8677", marginTop: 4 }}>{lang === "ar" ? r.dose : r.doseEn}</div>
              <div style={{ fontSize: 13, color: "#8a8677", marginTop: 2 }}>{t.prescribedBy} {lang === "ar" ? r.doctor : r.doctorEn}</div>
            </div>
          ))
        ) : (
          <div style={{ fontSize: 13, color: "#8a8677" }}>{t.rxNone}</div>
        )}
      </Card>
    </div>
  );
}

function CompanyDashboard({ t, lang, company, nameKey, areaKey, medNameKey, activeBranch, setActiveBranchId, onLogout, toggleStock, removeMed, addMed, newName, setNewName, newPrice, setNewPrice, addBranch, newBranchArea, setNewBranchArea }) {
  return (
    <div style={{ display: "flex", flexDirection: "column", gap: 18 }}>
      <Card>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
          <div>
            <div style={{ fontWeight: 800, fontSize: 17, fontFamily: "'Cairo','Inter', sans-serif" }}>{company[nameKey]}</div>
            <div style={{ fontSize: 13, color: "#8a8677", marginTop: 3, display: "flex", alignItems: "center", gap: 5 }}><Building2 size={13} /> {company.branches.length} {t.branches}</div>
          </div>
          <button onClick={onLogout} style={{ display: "flex", alignItems: "center", gap: 6, background: COLORS.sage, border: `1px solid ${COLORS.line}`, borderRadius: 10, padding: "8px 14px", fontSize: 13, fontWeight: 700, color: COLORS.deepAlt }}>
            <LogOut size={14} /> {t.logout}
          </button>
        </div>
        <div style={{ marginTop: 14, display: "flex", alignItems: "center", gap: 8, background: COLORS.sage, borderRadius: 10, padding: "8px 12px", width: "fit-content" }}>
          <CreditCard size={14} color={COLORS.deepAlt} />
          <span style={{ fontSize: 13, fontWeight: 700, color: COLORS.deepAlt }}>{t.subscription}: {company.plan === "monthly_5usd" ? (lang === "ar" ? "شهري - 5$" : "Monthly - $5") : company.plan}</span>
        </div>
      </Card>

      <Card>
        <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 10 }}>{t.branches}</div>
        <div style={{ display: "flex", gap: 8, flexWrap: "wrap", marginBottom: 14 }}>
          {company.branches.map((b) => (
            <button key={b.id} onClick={() => setActiveBranchId(b.id)} style={{ padding: "8px 14px", borderRadius: 999, fontSize: 13, fontWeight: 700, border: `1px solid ${activeBranch?.id === b.id ? COLORS.deepAlt : COLORS.line}`, background: activeBranch?.id === b.id ? COLORS.deepAlt : "#fff", color: activeBranch?.id === b.id ? "#fff" : COLORS.charcoal }}>{b[areaKey]}</button>
          ))}
        </div>
        <form onSubmit={addBranch} style={{ display: "flex", gap: 8 }}>
          <input value={newBranchArea} onChange={(e) => setNewBranchArea(e.target.value)} placeholder={t.newBranchPh} style={inputStyle} />
          <button type="submit" style={{ display: "flex", alignItems: "center", gap: 4, background: COLORS.sage, color: COLORS.deepAlt, border: `1px solid ${COLORS.line}`, borderRadius: 10, padding: "0 16px", fontWeight: 700, fontSize: 13, whiteSpace: "nowrap" }}><Plus size={15} /> {t.addBranch}</button>
        </form>
      </Card>

      {activeBranch && (
        <Card>
          <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 12 }}>{t.branchInv} {activeBranch[areaKey]}</div>
          <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
            {activeBranch.inventory.map((m) => (
              <div key={m.id} style={{ display: "flex", justifyContent: "space-between", alignItems: "center", border: `1px solid ${COLORS.line}`, borderRadius: 12, padding: "10px 12px" }}>
                <div>
                  <div style={{ fontWeight: 700, fontSize: 14 }}>{m[medNameKey]}</div>
                  <div style={{ fontSize: 12, color: "#8a8677", marginTop: 2 }}>{m.price.toLocaleString()} {lang === "ar" ? "ل.س" : "SYP"}</div>
                </div>
                <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
                  <button onClick={() => toggleStock(m.id)} style={{ border: "none", background: "transparent", padding: 0 }} aria-label="toggle stock"><StockBadge stock={m.stock} t={t} /></button>
                  <button onClick={() => removeMed(m.id)} style={{ border: "none", background: "transparent", color: COLORS.red, padding: 6, borderRadius: 8 }} aria-label="remove"><Trash2 size={16} /></button>
                </div>
              </div>
            ))}
            {activeBranch.inventory.length === 0 && <div style={{ fontSize: 13, color: "#8a8677" }}>{t.noMeds}</div>}
          </div>
          <form onSubmit={addMed} style={{ display: "flex", gap: 8, marginTop: 16, flexWrap: "wrap" }}>
            <input value={newName} onChange={(e) => setNewName(e.target.value)} placeholder={t.newMedPh} style={{ ...inputStyle, flex: 2, minWidth: 140 }} />
            <input value={newPrice} onChange={(e) => setNewPrice(e.target.value)} placeholder={t.price} type="number" style={{ ...inputStyle, flex: 1, minWidth: 90 }} />
            <button type="submit" style={{ display: "flex", alignItems: "center", gap: 4, background: COLORS.deepAlt, color: "#fff", border: "none", borderRadius: 10, padding: "0 16px", fontWeight: 700, fontSize: 13 }}><Plus size={15} /> {t.add}</button>
          </form>
        </Card>
      )}
    </div>
  );
}
