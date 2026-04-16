const fs = require("fs");
const path = require("path");

// пути относительно корня репозитория
const INPUT = path.resolve(__dirname, "../../tokens/tokens.json");
const OUTPUT = path.resolve(__dirname, "../../clean-tokens.json");

// рекурсивная очистка
function clean(obj) {
  if (Array.isArray(obj)) {
    return obj.map(clean);
  }

  if (obj && typeof obj === "object") {
    const newObj = {};

    for (const key in obj) {
      // ❌ удаляем фигмовские артефакты
      if (
        key === "$extensions" ||
        key.startsWith("com.figma")
      ) {
        continue;
      }

      newObj[key] = clean(obj[key]);
    }

    return newObj;
  }

  return obj;
}

try {
  const raw = JSON.parse(fs.readFileSync(INPUT, "utf8"));
  const cleaned = clean(raw);

  fs.writeFileSync(OUTPUT, JSON.stringify(cleaned, null, 2));
  console.log("✅ Tokens cleaned → clean-tokens.json");
} catch (error) {
  console.error("❌ Error cleaning tokens:", error);
  process.exit(1);
}
