<%*
const folder = "The Ember Index/fixations";
const files = app.vault.getMarkdownFiles().filter(f => f.path.startsWith(folder));
const random = files[Math.floor(Math.random() * files.length)];
const timestamp = tp.date.now("YYYY-MM-DD HH:mm");

const invokation = [
  "%% SCROLL-OF-INVOKATION-BEGINS %%",
  "## 🔥 Invoked Shard",
  "",
  `✨ [[${random.basename}]]`,
  `🕒 ${timestamp}`,
  "%% SCROLL-OF-INVOKATION-ENDS %%"
].join("\n");

const currentFile = tp.file.find_tfile(tp.file.title);
const content = await app.vault.read(currentFile);

const regex = /%% SCROLL-OF-INVOKATION-BEGINS %%[\s\S]*?%% SCROLL-OF-INVOKATION-ENDS %%/g;

let newContent = "";

if (regex.test(content)) {
  newContent = content.replace(regex, invokation.trim());
} else {
  newContent = content.trimEnd() + "\n\n" + invokation;
}

await app.vault.modify(currentFile, newContent);

// Registering in the register

const date = tp.date.now("YYYY-MM-DD HH:mm");
const logPath = "The Ember Index/register.md";

// Trova il file reale nel vault
const logFile = app.vault.getAbstractFileByPath(logPath);

if (logFile) {
  const logEntry = `### 📅 ${date}\Invoked: [[${random.basename}]]\n\n`;
  await app.vault.append(logFile, logEntry);
}

%>
