import React, { useState, useEffect, useCallback, useMemo } from "react";

const COLORS = {
  ivory: "#F4EEE2",
  ivorySoft: "#EFE7D6",
  ink: "#17140F",
  inkSoft: "#3A362D",
  gold: "#B08D4F",
  goldBright: "#C99F5D",
  terracotta: "#A24A2A",
  terracottaDeep: "#7E3A20",
  line: "rgba(23,20,15,0.12)",
  danger: "#A2352A",
};

const ADMIN_PIN = "1234";
const SETTINGS_KEY = "heidelitalia:settings";
const RESERVATIONS_KEY = "heidelitalia:reservations";

function buildTimeSlots() {
  const slots = [];
  for (let h = 12; h <= 21; h++) {
    slots.push(`${String(h).padStart(2, "0")}:00`);
    slots.push(`${String(h).padStart(2, "0")}:30`);
  }
  return slots;
}
const TIME_SLOTS = buildTimeSlots();

function toMinutes(time) {
  const [h, m] = time.split(":").map(Number);
  return h * 60 + m;
}
function formatEndTime(time) {
  const end = toMinutes(time) + 120;
  const h = Math.floor(end / 60) % 24;
  const m = end % 60;
  return `${String(h).padStart(2, "0")}:${String(m).padStart(2, "0")}`;
}
function formatDate(iso) {
  const d = new Date(iso + "T00:00:00");
  return d.toLocaleDateString("de-DE", { weekday: "short", day: "2-digit", month: "2-digit", year: "numeric" });
}
function todayISO() {
  return new Date().toISOString().slice(0, 10);
}
function overlapping(reservations, date, time, excludeId) {
  const start = toMinutes(time);
  const end = start + 120;
  return reservations.filter((r) => {
    if (r.id === excludeId) return false;
    if (r.date !== date) return false;
    const rStart = toMinutes(r.time);
    const rEnd = rStart + 120;
    return !(rEnd <= start || rStart >= end);
  });
}

function Field({ label, children }) {
  return (
    <label style={{ display: "flex", flexDirection: "column", gap: "0.4rem", fontSize: "0.82rem", fontWeight: 600, letterSpacing: "0.04em", color: COLORS.inkSoft }}>
      {label}
      {children}
    </label>
  );
}

const inputStyle = {
  padding: "0.75rem 0.9rem",
  fontSize: "0.95rem",
  border: `1px solid ${COLORS.line}`,
  borderRadius: "3px",
  background: "#fff",
  color: COLORS.ink,
  fontFamily: "'Manrope', sans-serif",
  outline: "none",
};

export default function HeidelitaliaReservierung() {
  const [view, setView] = useState("reserve");
  const [loading, setLoading] = useState(true);
  const [settings, setSettings] = useState({ tables: 10 });
  const [reservations, setReservations] = useState([]);

  const [name, setName] = useState("");
  const [people, setPeople] = useState(2);
  const [date, setDate] = useState(todayISO());
  const [time, setTime] = useState("19:00");
  const [formError, setFormError] = useState("");
  const [confirmation, setConfirmation] = useState(null);
  const [saving, setSaving] = useState(false);

  const [adminAuthed, setAdminAuthed] = useState(false);
  const [pinInput, setPinInput] = useState("");
  const [pinError, setPinError] = useState("");
  const [tablesInput, setTablesInput] = useState("10");
  const [settingsSaved, setSettingsSaved] = useState(false);
  const [storageError, setStorageError] = useState("");

  useEffect(() => {
    let cancelled = false;
    (async () => {
      try {
        const s = await window.storage.get(SETTINGS_KEY, true);
        if (!cancelled && s) {
          const parsed = JSON.parse(s.value);
          setSettings(parsed);
          setTablesInput(String(parsed.tables));
        }
      } catch (e) {
        try {
          await window.storage.set(SETTINGS_KEY, JSON.stringify({ tables: 10 }), true);
        } catch (e2) {
          setStorageError("Speicher konnte nicht initialisiert werden.");
        }
      }
      try {
        const r = await window.storage.get(RESERVATIONS_KEY, true);
        if (!cancelled && r) {
          setReservations(JSON.parse(r.value));
        }
      } catch (e) {
        try {
          await window.storage.set(RESERVATIONS_KEY, JSON.stringify([]), true);
        } catch (e2) {
          setStorageError("Speicher konnte nicht initialisiert werden.");
        }
      }
      if (!cancelled) setLoading(false);
    })();
    return () => {
      cancelled = true;
    };
  }, []);

  const saveReservations = useCallback(async (next) => {
    setReservations(next);
    try {
      const result = await window.storage.set(RESERVATIONS_KEY, JSON.stringify(next), true);
      if (!result) setStorageError("Änderung konnte nicht gespeichert werden. Bitte erneut versuchen.");
      else setStorageError("");
    } catch (e) {
      setStorageError("Änderung konnte nicht gespeichert werden. Bitte erneut versuchen.");
    }
  }, []);

  const availableForSelection = useMemo(() => {
    if (!date || !time) return settings.tables;
    return settings.tables - overlapping(reservations, date, time).length;
  }, [reservations, settings.tables, date, time]);

  async function handleSubmit(e) {
    e.preventDefault();
    setFormError("");
    setConfirmation(null);

    if (!name.trim()) {
      setFormError("Bitte geben Sie Ihren Namen ein.");
      return;
    }
    if (!people || people < 1 || people > 20) {
      setFormError("Bitte geben Sie eine gültige Personenzahl an (1–20).");
      return;
    }
    if (!date) {
      setFormError("Bitte wählen Sie ein Datum.");
      return;
    }
    if (date < todayISO()) {
      setFormError("Das Datum darf nicht in der Vergangenheit liegen.");
      return;
    }
    if (!time) {
      setFormError("Bitte wählen Sie eine Uhrzeit.");
      return;
    }

    const conflicting = overlapping(reservations, date, time);
    if (conflicting.length >= settings.tables) {
      setFormError(`Leider ist um ${time} Uhr kein Tisch mehr frei. Bitte wählen Sie eine andere Uhrzeit.`);
      return;
    }

    setSaving(true);
    const entry = {
      id: `${Date.now()}-${Math.random().toString(36).slice(2, 7)}`,
      name: name.trim(),
      people: Number(people),
      date,
      time,
      createdAt: new Date().toISOString(),
    };
    const next = [...reservations, entry];
    await saveReservations(next);
    setSaving(false);
    setConfirmation(entry);
    setName("");
    setPeople(2);
  }

  function handlePinSubmit(e) {
    e.preventDefault();
    if (pinInput === ADMIN_PIN) {
      setAdminAuthed(true);
      setPinError("");
      setPinInput("");
    } else {
      setPinError("Falscher PIN. Bitte erneut versuchen.");
    }
  }

  async function handleDeleteReservation(id) {
    const next = reservations.filter((r) => r.id !== id);
    await saveReservations(next);
  }

  async function handleSaveSettings(e) {
    e.preventDefault();
    const n = Number(tablesInput);
    if (!n || n < 1 || n > 200) return;
    const next = { tables: n };
    setSettings(next);
    try {
      const result = await window.storage.set(SETTINGS_KEY, JSON.stringify(next), true);
      if (result) {
        setSettingsSaved(true);
        setTimeout(() => setSettingsSaved(false), 2200);
      } else {
        setStorageError("Einstellungen konnten nicht gespeichert werden.");
      }
    } catch (e2) {
      setStorageError("Einstellungen konnten nicht gespeichert werden.");
    }
  }

  const sortedReservations = useMemo(() => {
    return [...reservations].sort((a, b) => (a.date + a.time).localeCompare(b.date + b.time));
  }, [reservations]);

  return (
    <div
      style={{
        fontFamily: "'Manrope', sans-serif",
        background: COLORS.ivory,
        color: COLORS.ink,
        minHeight: "100vh",
        width: "100%",
      }}
    >
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,500;1,9..144,500&family=Manrope:wght@400;500;600;700&display=swap');
        .hd-input:focus, .hd-select:focus { border-color: ${COLORS.gold} !important; box-shadow: 0 0 0 3px rgba(176,141,79,0.18); }
        .hd-btn { transition: transform .3s ease, filter .3s ease, background .3s ease, color .3s ease; }
        .hd-btn:hover { transform: translateY(-1px); }
        .hd-row:hover .hd-del { opacity: 1; }
        .hd-del { opacity: 0.55; transition: opacity .25s ease; }
        @media (max-width: 640px) {
          .hd-grid { grid-template-columns: 1fr !important; }
        }
      `}</style>

      {/* Header */}
      <div style={{ background: COLORS.ink, padding: "1.3rem 0" }}>
        <div style={{ maxWidth: 900, margin: "0 auto", padding: "0 1.5rem", display: "flex", alignItems: "center", justifyContent: "space-between", flexWrap: "wrap", gap: "1rem" }}>
          <div style={{ fontFamily: "'Fraunces', serif", fontStyle: "italic", fontSize: "1.4rem", color: COLORS.ivory }}>
            Heidel<span style={{ color: COLORS.goldBright, fontStyle: "normal" }}>italia</span>
          </div>
          <div style={{ display: "flex", gap: "0.6rem" }}>
            <button
              onClick={() => setView("reserve")}
              className="hd-btn"
              style={{
                padding: "0.6rem 1.2rem",
                fontSize: "0.78rem",
                letterSpacing: "0.06em",
                borderRadius: "2px",
                border: `1px solid ${view === "reserve" ? COLORS.goldBright : "rgba(244,238,226,0.3)"}`,
                background: view === "reserve" ? COLORS.goldBright : "transparent",
                color: view === "reserve" ? COLORS.ink : COLORS.ivory,
                fontWeight: 600,
              }}
            >
              Tisch reservieren
            </button>
            <button
              onClick={() => setView("admin")}
              className="hd-btn"
              style={{
                padding: "0.6rem 1.2rem",
                fontSize: "0.78rem",
                letterSpacing: "0.06em",
                borderRadius: "2px",
                border: `1px solid ${view === "admin" ? COLORS.goldBright : "rgba(244,238,226,0.3)"}`,
                background: view === "admin" ? COLORS.goldBright : "transparent",
                color: view === "admin" ? COLORS.ink : COLORS.ivory,
                fontWeight: 600,
              }}
            >
              Admin
            </button>
          </div>
        </div>
      </div>

      <div style={{ maxWidth: 900, margin: "0 auto", padding: "3rem 1.5rem 5rem" }}>
        {loading ? (
          <p style={{ color: COLORS.inkSoft }}>Daten werden geladen …</p>
        ) : view === "reserve" ? (
          <ReserveView
            name={name} setName={setName}
            people={people} setPeople={setPeople}
            date={date} setDate={setDate}
            time={time} setTime={setTime}
            formError={formError}
            confirmation={confirmation}
            availableForSelection={availableForSelection}
            totalTables={settings.tables}
            saving={saving}
            onSubmit={handleSubmit}
          />
        ) : !adminAuthed ? (
          <AdminGate pinInput={pinInput} setPinInput={setPinInput} pinError={pinError} onSubmit={handlePinSubmit} />
        ) : (
          <AdminView
            settings={settings}
            tablesInput={tablesInput}
            setTablesInput={setTablesInput}
            onSaveSettings={handleSaveSettings}
            settingsSaved={settingsSaved}
            reservations={sortedReservations}
            onDelete={handleDeleteReservation}
            storageError={storageError}
          />
        )}
      </div>
    </div>
  );
}

function ReserveView({ name, setName, people, setPeople, date, setDate, time, setTime, formError, confirmation, availableForSelection, totalTables, saving, onSubmit }) {
  return (
    <div>
      <p style={{ fontSize: "0.72rem", letterSpacing: "0.24em", textTransform: "uppercase", color: COLORS.terracottaDeep, fontWeight: 700, marginBottom: "0.6rem" }}>
        Reservierung
      </p>
      <h1 style={{ fontFamily: "'Fraunces', serif", fontSize: "clamp(1.9rem, 4vw, 2.6rem)", margin: "0 0 0.6rem" }}>
        Reservieren Sie Ihren Tisch
      </h1>
      <p style={{ color: COLORS.inkSoft, maxWidth: "56ch", marginBottom: "2.4rem" }}>
        Jede Reservierung ist für zwei Stunden vorgesehen. Der Tisch ist danach automatisch wieder frei verfügbar.
      </p>

      <form onSubmit={onSubmit} style={{ background: "#fff", border: `1px solid ${COLORS.line}`, borderRadius: "4px", padding: "2rem" }}>
        <div className="hd-grid" style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: "1.3rem", marginBottom: "1.3rem" }}>
          <Field label="Name">
            <input className="hd-input" style={inputStyle} type="text" value={name} onChange={(e) => setName(e.target.value)} placeholder="Ihr Name" />
          </Field>
          <Field label="Anzahl Personen">
            <input className="hd-input" style={inputStyle} type="number" min="1" max="20" value={people} onChange={(e) => setPeople(e.target.value)} />
          </Field>
        </div>
        <div className="hd-grid" style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: "1.3rem", marginBottom: "1rem" }}>
          <Field label="Datum">
            <input className="hd-input" style={inputStyle} type="date" min={todayISO()} value={date} onChange={(e) => setDate(e.target.value)} />
          </Field>
          <Field label="Uhrzeit">
            <select className="hd-select" style={inputStyle} value={time} onChange={(e) => setTime(e.target.value)}>
              {TIME_SLOTS.map((t) => (
                <option key={t} value={t}>{t} Uhr</option>
              ))}
            </select>
          </Field>
        </div>

        <p style={{ fontSize: "0.85rem", color: availableForSelection > 0 ? COLORS.inkSoft : COLORS.danger, marginBottom: "1.4rem" }}>
          {availableForSelection > 0
            ? `${availableForSelection} von ${totalTables} Tischen um ${time} Uhr verfügbar · Tisch frei ab ${formatEndTime(time)} Uhr`
            : `Um ${time} Uhr sind aktuell keine Tische mehr frei.`}
        </p>

        {formError && (
          <p style={{ background: "rgba(162,53,42,0.08)", color: COLORS.danger, padding: "0.85rem 1rem", borderRadius: "3px", fontSize: "0.88rem", marginBottom: "1.2rem" }}>
            {formError}
          </p>
        )}

        <button
          type="submit"
          disabled={saving}
          className="hd-btn"
          style={{
            width: "100%",
            padding: "1rem",
            fontSize: "0.85rem",
            fontWeight: 700,
            letterSpacing: "0.1em",
            textTransform: "uppercase",
            border: "none",
            borderRadius: "2px",
            background: `linear-gradient(135deg, ${COLORS.goldBright}, ${COLORS.gold})`,
            color: COLORS.ink,
            opacity: saving ? 0.7 : 1,
          }}
        >
          {saving ? "Wird reserviert …" : "Tisch reservieren"}
        </button>
      </form>

      {confirmation && (
        <div style={{ marginTop: "1.6rem", background: COLORS.ink, color: COLORS.ivory, padding: "1.6rem 2rem", borderRadius: "4px" }}>
          <p style={{ fontFamily: "'Fraunces', serif", fontStyle: "italic", fontSize: "1.2rem", marginBottom: "0.5rem", color: COLORS.goldBright }}>
            Reservierung bestätigt
          </p>
          <p style={{ fontSize: "0.95rem", lineHeight: 1.7 }}>
            {confirmation.name}, Tisch für {confirmation.people} {confirmation.people === 1 ? "Person" : "Personen"} am {formatDate(confirmation.date)} um {confirmation.time} Uhr.
            <br />Der Tisch ist bis {formatEndTime(confirmation.time)} Uhr reserviert.
          </p>
        </div>
      )}
    </div>
  );
}

function AdminGate({ pinInput, setPinInput, pinError, onSubmit }) {
  return (
    <div style={{ maxWidth: 380 }}>
      <p style={{ fontSize: "0.72rem", letterSpacing: "0.24em", textTransform: "uppercase", color: COLORS.terracottaDeep, fontWeight: 700, marginBottom: "0.6rem" }}>
        Admin-Bereich
      </p>
      <h1 style={{ fontFamily: "'Fraunces', serif", fontSize: "1.9rem", margin: "0 0 1.6rem" }}>Anmelden</h1>
      <form onSubmit={onSubmit} style={{ background: "#fff", border: `1px solid ${COLORS.line}`, borderRadius: "4px", padding: "1.8rem" }}>
        <Field label="PIN">
          <input
            className="hd-input"
            style={inputStyle}
            type="password"
            inputMode="numeric"
            value={pinInput}
            onChange={(e) => setPinInput(e.target.value)}
            placeholder="••••"
            autoFocus
          />
        </Field>
        {pinError && <p style={{ color: COLORS.danger, fontSize: "0.85rem", marginTop: "0.8rem" }}>{pinError}</p>}
        <button
          type="submit"
          className="hd-btn"
          style={{ marginTop: "1.4rem", width: "100%", padding: "0.9rem", fontSize: "0.82rem", fontWeight: 700, letterSpacing: "0.08em", textTransform: "uppercase", border: "none", borderRadius: "2px", background: COLORS.ink, color: COLORS.ivory }}
        >
          Anmelden
        </button>
      </form>
      <p style={{ fontSize: "0.78rem", color: COLORS.inkSoft, marginTop: "1rem" }}>
        Hinweis: Dieser PIN-Schutz dient nur als einfache Zugangssperre für die Demo und ersetzt keine echte Authentifizierung.
      </p>
    </div>
  );
}

function AdminView({ settings, tablesInput, setTablesInput, onSaveSettings, settingsSaved, reservations, onDelete, storageError }) {
  return (
    <div>
      <p style={{ fontSize: "0.72rem", letterSpacing: "0.24em", textTransform: "uppercase", color: COLORS.terracottaDeep, fontWeight: 700, marginBottom: "0.6rem" }}>
        Admin-Bereich
      </p>
      <h1 style={{ fontFamily: "'Fraunces', serif", fontSize: "clamp(1.9rem, 4vw, 2.6rem)", margin: "0 0 2rem" }}>
        Reservierungen verwalten
      </h1>

      {storageError && (
        <p style={{ background: "rgba(162,53,42,0.08)", color: COLORS.danger, padding: "0.85rem 1rem", borderRadius: "3px", fontSize: "0.88rem", marginBottom: "1.4rem" }}>
          {storageError}
        </p>
      )}

      <div style={{ background: "#fff", border: `1px solid ${COLORS.line}`, borderRadius: "4px", padding: "1.6rem 1.8rem", marginBottom: "2.2rem" }}>
        <h3 style={{ fontFamily: "'Fraunces', serif", fontSize: "1.15rem", marginBottom: "1rem" }}>Anzahl der Tische</h3>
        <form onSubmit={onSaveSettings} style={{ display: "flex", gap: "0.9rem", alignItems: "flex-end", flexWrap: "wrap" }}>
          <Field label="Tische im Restaurant">
            <input className="hd-input" style={{ ...inputStyle, width: "140px" }} type="number" min="1" max="200" value={tablesInput} onChange={(e) => setTablesInput(e.target.value)} />
          </Field>
          <button type="submit" className="hd-btn" style={{ padding: "0.75rem 1.4rem", fontSize: "0.8rem", fontWeight: 700, letterSpacing: "0.06em", textTransform: "uppercase", border: "none", borderRadius: "2px", background: COLORS.goldBright, color: COLORS.ink }}>
            Speichern
          </button>
          {settingsSaved && <span style={{ fontSize: "0.85rem", color: COLORS.terracottaDeep }}>Gespeichert.</span>}
        </form>
        <p style={{ fontSize: "0.82rem", color: COLORS.inkSoft, marginTop: "0.9rem" }}>
          Aktuell konfiguriert: {settings.tables} {settings.tables === 1 ? "Tisch" : "Tische"}.
        </p>
      </div>

      <div style={{ background: "#fff", border: `1px solid ${COLORS.line}`, borderRadius: "4px", padding: "1.6rem 1.8rem" }}>
        <h3 style={{ fontFamily: "'Fraunces', serif", fontSize: "1.15rem", marginBottom: "1.2rem" }}>
          Alle Reservierungen ({reservations.length})
        </h3>
        {reservations.length === 0 ? (
          <p style={{ color: COLORS.inkSoft, fontSize: "0.92rem" }}>Noch keine Reservierungen vorhanden.</p>
        ) : (
          <div style={{ display: "flex", flexDirection: "column" }}>
            {reservations.map((r) => (
              <div
                key={r.id}
                className="hd-row"
                style={{
                  display: "flex",
                  alignItems: "center",
                  justifyContent: "space-between",
                  gap: "1rem",
                  padding: "0.9rem 0",
                  borderBottom: `1px solid ${COLORS.line}`,
                  flexWrap: "wrap",
                }}
              >
                <div>
                  <p style={{ fontFamily: "'Fraunces', serif", fontSize: "1.02rem" }}>{r.name}</p>
                  <p style={{ fontSize: "0.82rem", color: COLORS.inkSoft }}>
                    {formatDate(r.date)} · {r.time}–{formatEndTime(r.time)} Uhr · {r.people} {r.people === 1 ? "Person" : "Personen"}
                  </p>
                </div>
                <button
                  onClick={() => onDelete(r.id)}
                  className="hd-btn hd-del"
                  style={{
                    padding: "0.55rem 1rem",
                    fontSize: "0.76rem",
                    fontWeight: 700,
                    letterSpacing: "0.06em",
                    textTransform: "uppercase",
                    border: `1px solid ${COLORS.danger}`,
                    borderRadius: "2px",
                    background: "transparent",
                    color: COLORS.danger,
                  }}
                >
                  Löschen
                </button>
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}
