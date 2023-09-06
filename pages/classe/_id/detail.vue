<template>
  <section>
    <div>
      <h1 class="title-cam">Caméra de la salle A</h1>
      <div class="btn-cam">
        <button id="toggleVideoEl" onclick="toggleVideo()">Cacher la vidéo</button>
        <button id="toggleDetectingEl" onclick="toggleDetecting()">Commencer la détection</button>
      </div>
      <div class="number-cam">
        <p>Personnes détectées : <span id="personCount">0</span><br></p>
        <p>Chaises détectées : <span id="chairCount">0</span><br></p>
      </div>
    </div>

    <div id="modal" class="modal">
      <div class="modal-content">
        <h2>Authentification requise</h2>
        <p>Veuillez saisir le mot de passe :</p>
        <input type="password" id="passwordInput">
        <button id="authenticateButton">Valider</button>
      </div>
    </div>
  </section>

</template>

<script>
import {onMounted} from "vue";

onMounted(() => {
  // Définir le curseur en attente jusqu'à ce que l'élément vidéo soit chargé
  document.body.style.cursor = 'wait';
  // Déclarations des variables globales
  let video = null; // élément vidéo
  let detector = null; // objet détecteur
  let detections = []; // stocke les résultats de détection
  let videoVisibility = true;
  let detecting = false;
  let personCount = 0;
  let chairCount = 0;
  const correctPassword = '123456'; // Mot de passe requis

// Éléments HTML globaux
  const toggleVideoEl = document.getElementById('toggleVideoEl');
  const toggleDetectingEl = document.getElementById('toggleDetectingEl');
  const personCountEl = document.getElementById('personCount');
  const chairCountEl = document.getElementById('chairCount');
  const modal = document.getElementById('modal');
  const passwordInput = document.getElementById('passwordInput');
  const authenticateButton = document.getElementById('authenticateButton');

// Définir le curseur en attente jusqu'à ce que l'élément vidéo soit chargé
  document.body.style.cursor = 'wait';

// Fonction preload() : appelée avant setup()
  function preload() {
    // Crée un objet détecteur à partir du modèle "cocossd"
    detector = ml5.objectDetector('cocossd');
    console.log('Objet détecteur est chargé');
  }

// Fonction setup() : appelée une fois au démarrage du programme
  function setup() {
    // Crée un élément canvas avec une largeur de 640 et une hauteur de 480 pixels
    createCanvas(640, 480);

    // Crée un nouvel élément <video> HTML5 qui contient le flux audio/vidéo de la webcam.
    // L'élément est distinct du canvas et est affiché par défaut.
    video = createCapture(VIDEO);
    video.size(640, 480);
    console.log('Élément vidéo est créé');

    // Écoute l'événement 'loadeddata' pour l'élément vidéo
    video.elt.addEventListener('loadeddata', function () {
      // Rétablit le curseur par défaut une fois que l'élément vidéo est prêt
      if (video.elt.readyState >= 2) {
        document.body.style.cursor = 'default';
        console.log('L\'élément vidéo est prêt ! Cliquez sur "Commencer la détection" pour voir la magie !');
      }
    });
  }

// Fonction toggleVideo() : pour afficher ou masquer la vidéo
  function toggleVideo() {
    if (!video) return;
    if (videoVisibility) {
      video.hide();
      toggleVideoEl.innerText = 'Voir la vidéo';
    } else {
      video.show();
      toggleVideoEl.innerText = 'Cacher la vidéo';
    }
    videoVisibility = !videoVisibility;
  }

// Fonction toggleDetecting() : pour commencer ou arrêter la détection
  function toggleDetecting() {
    if (!video || !detector) return;
    if (!detecting) {
      showModal(); // Afficher la boîte modale pour l'authentification
    } else {
      toggleDetectingEl.innerText = 'Commencer la détection';
      detecting = false;
    }
  }

// Fonction pour afficher la boîte modale
  function showModal() {
    modal.style.display = 'block';
  }

// Fonction pour cacher la boîte modale
  function hideModal() {
    modal.style.display = 'none';
    passwordInput.value = ''; // Effacez le champ de mot de passe
  }

// Écouteur d'événement pour le bouton "Valider" de la boîte modale
  authenticateButton.addEventListener('click', function () {
    const enteredPassword = passwordInput.value;
    if (enteredPassword === correctPassword) {
      hideModal();
      detect(); // Démarrer la détection si le mot de passe est correct
      toggleDetectingEl.innerText = 'Arrêter la détection';
      detecting = true;
    } else {
      alert('Mot de passe incorrect. Veuillez réessayer.');
    }
  });

// Écouteur d'événement pour le bouton "Commencer la détection"
  toggleDetectingEl.addEventListener('click', toggleDetecting);

// Cacher la boîte modale lorsque l'utilisateur clique à l'extérieur de celle-ci
  window.addEventListener('click', function (event) {
    if (event.target === modal) {
      hideModal();
    }
  });

// Fonction detect() : pour démarrer la détection
  function detect() {
    // Instruit l'objet "detector" à détecter les objets à partir de l'élément vidéo
    // et à appeler la fonction "onDetected" lorsqu'un objet est détecté
    detector.detect(video, onDetected);
  }

// Fonction callback "onDetected" : appelée lorsqu'un objet est détecté
  function onDetected(error, results) {
    if (error) {
      console.error(error);
    }
    detections = results;
    personCount = 0;
    chairCount = 0;

    // Compter le nombre de personnes et de chaises en fonction de l'étiquette
    for (let i = 0; i < detections.length; i++) {
      const object = detections[i];
      if (object.label === "person") {
        personCount++;
      } else if (object.label === "chair") {
        chairCount++;
      }
    }

    // Mettre à jour les comptages affichés sur la page web
    personCountEl.innerText = personCount;
    chairCountEl.innerText = chairCount;

    // Continuer la détection des objets si le drapeau de détection est vrai
    if (detecting) {
      detect();
    }
  }

// Fonction draw() : s'exécute en continu jusqu'à ce que noLoop() soit appelé
  function draw() {
    if (!video || !detecting) return;
    // Dessine le cadre vidéo sur le canvas en haut à gauche
    image(video, 0, 0);

    // Dessine tous les objets détectés sur le canvas
    for (let i = 0; i < detections.length; i++) {
      drawResult(detections[i]);
    }
  }

// Fonction drawResult() : dessine un résultat de détection
  function drawResult(object) {
    drawBoundingBox(object);
    drawLabel(object);
  }

// Fonction drawBoundingBox() : dessine une boîte autour de l'objet détecté
  function drawBoundingBox(object) {
    stroke('green'); // Couleur du tracé
    strokeWeight(4); // Épaisseur du tracé
    noFill(); // Désactiver le remplissage
    rect(object.x, object.y, object.width, object.height); // Dessiner un rectangle
  }

// Fonction drawLabel() : dessine l'étiquette de l'objet détecté (à l'intérieur de la boîte)
  function drawLabel(object) {
    noStroke(); // Désactiver le tracé
    fill('white'); // Couleur de remplissage
    textSize(24); // Taille de la police
    text(object.label, object.x + 10, object.y + 24); // Dessiner du texte
  }

});

</script>


<style>
.title-cam {
  text-align: center;
  margin-bottom: 2rem;
}

.btn-cam {
  text-align: center;
  margin-bottom: 1rem;
}

#toggleVideoEl {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-right: 10px;
  padding-left: 10px;
  border-radius: 5px;
  border: none;
  background-color: #2EB8A8;
  color: #fff;
  cursor: pointer;
}

#toggleDetectingEl {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-right: 10px;
  padding-left: 10px;
  border-radius: 5px;
  border: none;
  background-color: #2EB8A8;
  color: #fff;
  cursor: pointer;
}

.number-cam {
  display: flex;
  gap: 2rem;
  justify-content: center;
  margin-bottom: 1rem;
}

/* Styles pour la boîte modale */
.modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
}

.modal-content {
  background-color: #fff;
  text-align: center;
  padding: 20px;
  width: 300px;
  margin: 100px auto;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.3);
}
</style>
