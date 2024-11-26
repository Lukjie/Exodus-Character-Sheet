<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Exodus Dominion Character Sheet</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Montserrat&display=swap');

        body {
            font-family: 'Montserrat', sans-serif;
            background-color: #0D0D0D;
            color: #E6E6E6;
            margin: 0;
            padding: 0;
        }
        h1, h2, h3 {
            font-family: 'Cinzel', serif;
            color: #D4AF37;
            text-align: center;
            text-transform: uppercase;
            letter-spacing: 2px;
        }
        /* Container and Tab Styles */
        .container {
            max-width: 900px;
            margin: auto;
            background-color: #1A1A1A;
            padding: 0 20px 20px 20px;
            border: 2px solid #800000;
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.5);
        }
        .tab {
            overflow: hidden;
            border-bottom: 1px solid #800000;
            display: flex;
        }
        .tab button {
            background-color: inherit;
            border: none;
            outline: none;
            cursor: pointer;
            padding: 14px 16px;
            transition: 0.3s;
            font-size: 18px;
            color: #D4AF37;
            flex: 1;
            font-family: 'Cinzel', serif;
        }
        .tab button:hover {
            background-color: #800000;
        }
        .tab button.active {
            background-color: #800000;
            color: #fff;
        }
        .tabcontent {
            display: none;
            padding: 20px 0;
        }
        /* Section Styles */
        .section {
            border: 1px solid #333;
            padding: 15px;
            margin-bottom: 20px;
            background-color: #262626;
            position: relative;
        }
        .section h2 {
            margin-top: 0;
            background-color: #800000;
            padding: 10px;
            color: #fff;
            border-radius: 5px;
        }
        /* Form Styles */
        label {
            display: block;
            margin-bottom: 5px;
            color: #FFD700;
            font-weight: bold;
        }
        input[type="text"], input[type="number"], select, textarea, input[type="file"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            background-color: #333;
            color: #E6E6E6;
            border: 1px solid #4D4D4D;
            border-radius: 5px;
        }
        textarea {
            resize: vertical;
            height: 100px;
        }
        .attributes, .abilities, .equipment {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
        .attributes div, .abilities div, .equipment div {
            flex: 1 1 48%;
            margin-bottom: 20px;
        }
        .abilities div, .equipment div {
            flex: 1 1 100%;
        }
        /* Button Styles */
        .submit-btn {
            display: block;
            width: 100%;
            padding: 15px;
            background-color: #D4AF37;
            border: none;
            color: #0D0D0D;
            font-size: 18px;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 1px;
            border-radius: 5px;
            box-shadow: 0 5px 10px rgba(212, 175, 55, 0.3);
        }
        .submit-btn:hover {
            background-color: #FFD700;
        }
        /* Header Image */
        .header-image {
            width: 100%;
            height: 200px;
            background: url('https://i.postimg.cc/wvJr9F1p/TED-Transparent.png') no-repeat center center;
            background-size: contain;
            margin-bottom: 0;
            border-bottom: 2px solid #800000;
        }
        /* Sith Code Image */
        .sith-code-image {
            width: 100%;
            height: auto;
            margin: 20px 0;
            border: 2px solid #800000;
            border-radius: 5px;
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.5);
        }
        /* Art Gallery Styles */
        .art-gallery {
            text-align: center;
            padding: 20px;
        }
        .art-gallery img {
            max-width: 100%;
            height: auto;
            margin: 20px 0;
            border: 2px solid #800000;
            border-radius: 5px;
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.5);
        }
        /* Scrollbar Styling */
        ::-webkit-scrollbar {
            width: 10px;
        }
        ::-webkit-scrollbar-track {
            background: #1A1A1A;
        }
        ::-webkit-scrollbar-thumb {
            background: #800000;
            border-radius: 5px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #D4AF37;
        }
        /* Responsive Design */
        @media (max-width: 600px) {
            .attributes div, .abilities div, .equipment div {
                flex: 1 1 100%;
            }
            .container {
                padding: 0 10px 10px 10px;
            }
            .tab button {
                font-size: 16px;
            }
        }
    </style>
    <script>
        function openTab(evt, tabName) {
            var i, tabcontent, tablinks;

            // Hide all tab content
            tabcontent = document.getElementsByClassName("tabcontent");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
            }

            // Remove the active class from all tabs
            tablinks = document.getElementsByClassName("tablinks");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].className = tablinks[i].className.replace(" active", "");
            }

            // Show the current tab and add an active class to the button
            document.getElementById(tabName).style.display = "block";
            evt.currentTarget.className += " active";
        }

        // Open the Character Sheet tab by default
        window.onload = function() {
            document.getElementById("defaultOpen").click();
            loadCharacterData();
        }

        // Load character data from localStorage (if any)
        function loadCharacterData() {
            var savedData = localStorage.getItem('characterData');
            if (savedData) {
                var data = JSON.parse(savedData);
                for (var key in data) {
                    if (data.hasOwnProperty(key)) {
                        var field = document.querySelector('[name="' + key + '"]');
                        if (field) field.value = data[key];
                    }
                }
            }
        }

        // Save character data to localStorage
        function saveCharacterData() {
            var form = document.querySelector('form');
            var formData = new FormData(form);
            var data = {};
            formData.forEach(function(value, key) {
                data[key] = value;
            });
            localStorage.setItem('characterData', JSON.stringify(data));
            alert('Character sheet saved!');
        }

        // Handle image upload and display
        function previewImage(event) {
            var reader = new FileReader();
            reader.onload = function() {
                var output = document.getElementById('character-art');
                output.src = reader.result;
                output.style.display = 'block';
            }
            if (event.target.files && event.target.files[0]) {
                reader.readAsDataURL(event.target.files[0]);
            }
        }
    </script>
</head>
<body>
    <div class="container">
        <!-- Header Image -->
        <div class="header-image"></div>

        <!-- Tab Navigation -->
        <div class="tab">
            <button class="tablinks" onclick="openTab(event, 'CharacterSheet')" id="defaultOpen">Character Sheet</button>
            <button class="tablinks" onclick="openTab(event, 'SithCode')">Sith Code</button>
            <button class="tablinks" onclick="openTab(event, 'ArtGallery')">Art Gallery</button>
        </div>

        <!-- Character Sheet Content -->
        <div id="CharacterSheet" class="tabcontent">
            <h1>Exodus Dominion Character Sheet</h1>
            <form onsubmit="saveCharacterData(); return false;">
                <!-- Personal Information Section -->
                <div class="section">
                    <h2>Personal Information</h2>
                    <label for="character-name">Character Name</label>
                    <input type="text" id="character-name" name="character-name">

                    <label for="player-name">Player Name</label>
                    <input type="text" id="player-name" name="player-name">

                    <label for="species">Species</label>
                    <input type="text" id="species" name="species">

                    <label for="background">Background</label>
                    <textarea id="background" name="background"></textarea>

                    <label for="role">Role/Rank</label>
                    <select id="role" name="role">
                        <option value="acolyte">Acolyte (Levels 1-5)</option>
                        <option value="apprentice">Apprentice (Levels 6-9)</option>
                        <option value="lord">Lord (Levels 10-14)</option>
                        <option value="high-lord">High Lord (Levels 15-18)</option>
                        <option value="darth">Darth (Levels 19-20)</option>
                        <option value="unique">Unique Role (Specify Below)</option>
                    </select>

                    <label for="unique-role">If Unique Role, Specify</label>
                    <input type="text" id="unique-role" name="unique-role">
                </div>

                <!-- Attributes Section -->
                <div class="section">
                    <h2>Attributes</h2>
                    <div class="attributes">
                        <div>
                            <label for="level">Level</label>
                            <input type="number" id="level" name="level" min="1" max="20">
                        </div>
                        <div>
                            <label for="hp">Hit Points (HP)</label>
                            <input type="number" id="hp" name="hp">
                        </div>
                        <div>
                            <label for="attack-bonus">Attack Bonus</label>
                            <input type="number" id="attack-bonus" name="attack-bonus">
                        </div>
                        <div>
                            <label for="defense-class">Defense Class (DC)</label>
                            <input type="number" id="defense-class" name="defense-class">
                        </div>
                    </div>
                </div>

                <!-- Force Abilities Section -->
                <div class="section">
                    <h2>Force Abilities</h2>
                    <label for="force-form">Force Form</label>
                    <select id="force-form" name="force-form">
                        <option value="force-channel">Force Channel</option>
                        <option value="force-affinity">Force Affinity</option>
                        <option value="force-mastery">Force Mastery</option>
                        <option value="force-potency">Force Potency</option>
                    </select>

                    <div class="abilities">
                        <div>
                            <label for="abilities">List of Abilities</label>
                            <textarea id="abilities" name="abilities" placeholder="E.g., Telekinesis, Force Lightning..."></textarea>
                        </div>
                    </div>
                </div>

                <!-- Equipment Section -->
                <div class="section">
                    <h2>Equipment</h2>
                    <div class="equipment">
                        <div>
                            <label for="weapon">Primary Weapon</label>
                            <input type="text" id="weapon" name="weapon" placeholder="E.g., Lightsaber, Blaster...">
                        </div>
                        <div>
                            <label for="weapon-damage">Weapon Damage</label>
                            <input type="text" id="weapon-damage" name="weapon-damage" placeholder="E.g., 1d30, 2d50...">
                        </div>
                        <div>
                            <label for="armor">Armor</label>
                            <input type="text" id="armor" name="armor">
                        </div>
                        <div>
                            <label for="additional-equipment">Additional Equipment</label>
                            <textarea id="additional-equipment" name="additional-equipment"></textarea>
                        </div>
                    </div>
                </div>

                <!-- Background and Notes Section -->
                <div class="section">
                    <h2>Background and Notes</h2>
                    <label for="character-story">Character Story</label>
                    <textarea id="character-story" name="character-story"></textarea>

                    <label for="notes">Additional Notes</label>
                    <textarea id="notes" name="notes"></textarea>
                </div>

                <!-- Submit Button -->
                <button type="submit" class="submit-btn">Save Character Sheet</button>
            </form>
        </div>

        <!-- Sith Code Content -->
        <div id="SithCode" class="tabcontent">
            <h1>The Sith Code</h1>
            <img src="https://i.postimg.cc/VkCZqDFb/wp10326445.png" alt="Sith Code" class="sith-code-image">
        </div>

        <!-- Art Gallery Content -->
        <div id="ArtGallery" class="tabcontent">
            <h1>Art Gallery</h1>
            <div class="art-gallery">
                <label for="art-upload">Upload Your Character Art</label>
                <input type="file" id="art-upload" accept="image/*" onchange="previewImage(event)">
                <img id="character-art" src="#" alt="Your Character Art" style="display:none;">
            </div>
        </div>
    </div>
</body>
</html>
