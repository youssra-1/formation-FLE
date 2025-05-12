site_structure = {
    "index.html": """
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Formation FLE - Microlearning</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Apprendre le français avec le microlearning</h1>
    <p>Objectifs de la formation :
      <ul>
        <li>Améliorer la compréhension orale</li>
        <li>Acquérir du vocabulaire essentiel</li>
        <li>Pratiquer la grammaire de manière interactive</li>
        <li>Apprendre à son rythme, partout</li>
      </ul>
    </p>
    <video controls width="600">
      <source src="teaser.mp4" type="video/mp4">
      Votre navigateur ne supporte pas la lecture vidéo.
    </video>
    <a href="module1.html" class="start-btn">Commencer la formation</a>
  </div>
</body>
</html>
""",
    "style.css": """
body {
  font-family: Arial, sans-serif;
  background: #f9f9f9;
  color: #333;
  padding: 20px;
}

.container {
  max-width: 800px;
  margin: auto;
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

a.start-btn, a.next-btn {
  display: inline-block;
  margin-top: 20px;
  padding: 10px 20px;
  background: #007BFF;
  color: white;
  text-decoration: none;
  border-radius: 5px;
}
""",
    "quiz.js": """
function checkQuiz(formId, correctAnswer, scoreId) {
  const form = document.getElementById(formId);
  const selected = form.querySelector('input[name=\"q1\"]:checked');
  const result = document.getElementById(scoreId);
  if (!selected) {
    result.textContent = "Veuillez choisir une réponse.";
    return;
  }
  if (selected.value === correctAnswer) {
    result.textContent = "✅ Bonne réponse ! Score : 1/1";
  } else {
    result.textContent = "❌ Mauvaise réponse. Score : 0/1";
  }
}
""",
    "feedback.html": """
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Feedback</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Contactez-nous ou laissez un avis</h2>
    <form action="https://formspree.io/f/TON-CODE" method="POST">
      <label for="name">Nom :</label><br>
      <input type="text" name="name" required><br>
      <label for="message">Message :</label><br>
      <textarea name="message" required></textarea><br>
      <button type="submit">Envoyer</button>
    </form>
  </div>
</body>
</html>
"""
}

# Ajouter les fichiers des 7 modules
for i in range(1, 8):
    filename = f"module{i}.html"
    site_structure[filename] = f"""
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Module {i}</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Module {i} : Titre du module</h2>
    <video controls width="600">
      <source src="videos/module{i}.mp4" type="video/mp4">
      Votre navigateur ne supporte pas la lecture vidéo.
    </video>
    <div class="quiz">
      <h3>Quiz : Choisissez la bonne réponse</h3>
      <form id="quiz{i}">
        <p>Exemple de question ?</p>
        <input type="radio" name="q1" value="a"> Réponse A<br>
        <input type="radio" name="q1" value="b"> Réponse B<br>
        <input type="radio" name="q1" value="c"> Réponse C<br><br>
        <button type="button" onclick="checkQuiz('quiz{i}', 'b', 'score{i}')">Soumettre</button>
        <p id="score{i}"></p>
      </form>
    </div>
    <a href="module{i+1}.html" class="next-btn">Module suivant</a>
  </div>
  <script src="quiz.js"></script>
</body>
</html>
""" if i < 7 else """
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Module 7</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h2>Module 7 : Révision et évaluation</h2>
    <video controls width="600">
      <source src="videos/module7.mp4" type="video/mp4">
      Votre navigateur ne supporte pas la lecture vidéo.
    </video>
    <div class="quiz">
      <h3>Quiz : Choisissez la bonne réponse</h3>
      <form id="quiz7">
        <p>Exemple de question ?</p>
        <input type="radio" name="q1" value="a"> Réponse A<br>
        <input type="radio" name="q1" value="b"> Réponse B<br>
        <input type="radio" name="q1" value="c"> Réponse C<br><br>
        <button type="button" onclick="checkQuiz('quiz7', 'b', 'score7')">Soumettre</button>
        <p id="score7"></p>
      </form>
    </div>
    <a href="index.html" class="next-btn">Retour à l'accueil</a>
  </div>
  <script src="quiz.js"></script>
</body>
</html>
"""

# Créer le fichier ZIP
zip_path = "/mnt/data/site_fle_microlearning.zip"
with zipfile.ZipFile(zip_path, 'w') as zipf:
    for filename, content in site_structure.items():
        zipf.writestr(filename, content)
