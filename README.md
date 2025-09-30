# 1-events-and-catering<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>Events & Catering All Packages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9faff;
      padding: 25px 15px;
      color: #222;
      max-width: 980px;
      margin: auto;
    }

    h1,
    h2 {
      text-align: center;
      margin-bottom: 25px;
    }

    nav {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-bottom: 30px;
    }

    nav button {
      border-radius: 7px;
      border: none;
      padding: 12px 24px;
      background: #eef2ff;
      color: #335acc;
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      transition: background 0.3s, color 0.3s;
    }

    nav button.selected {
      background: #335acc;
      color: white;
    }

    section {
      display: none;
      background: white;
      border-radius: 14px;
      box-shadow: 0 3px 12px #cbd3e3bf;
      padding: 22px 32px;
      margin-bottom: 30px;
    }

    section.active {
      display: block;
    }

    .btn-group {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 14px;
      margin-bottom: 24px;
    }

    .btn-group button {
      border-radius: 10px;
      border: none;
      padding: 16px 24px;
      background: #f0f4ff;
      color: #3366cc;
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      min-width: 140px;
      transition: background 0.25s, color 0.25s;
      white-space: nowrap;
    }

    .btn-group button.selected {
      background: #335acc;
      color: #fff;
    }

    .services-list {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 12px;
      margin-bottom: 18px;
    }

    .services-list button {
      min-width: 160px;
      background: #eef5fc;
      color: #2a437d;
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      border: none;
      border-radius: 9px;
      padding: 14px 18px;
      box-shadow: 0 2px 6px #3952bc30;
      transition: background 0.3s, color 0.3s;
      white-space: nowrap;
    }

    .services-list button.selected {
      background: #335acc;
      color: white;
    }

    .summary {
      font-weight: 600;
      font-size: 1.1rem;
      background: #dadedf;
      border-radius: 12px;
      padding: 20px;
      min-height: 60px;
    }

    ul {
      padding-left: 25px;
    }

    @media (max-width: 650px) {
      .btn-group {
        flex-direction: column;
        align-items: center;
      }

      .btn-group button,
      .services-list button {
        min-width: 100%;
      }
    }
  </style>
</head>

<body>
  <h1>All Packages & Services - Events & Catering</h1>

  <nav>
    <button id="navPackage" class="selected" onclick="switchSection('package')">Package</button>
    <button id="navCustomized" onclick="switchSection('customized')">Customized</button>
    <button id="navPremium" onclick="switchSection('premium')">Premium</button>
  </nav>

  <!-- Package Section -->
  <section id="packageSection" class="active">
    <h2>Choose Your Package</h2>
    <div class="btn-group" id="packageButtons"></div>

    <h3>Package Services</h3>
    <div class="services-list" id="packageServices"></div>
  </section>

  <!-- Customized Section -->
  <section id="customizedSection">
    <h2>Customize Your Event</h2>
    <div class="btn-group" id="customEventTypes"></div>

    <h3>Customized Services</h3>
    <div class="services-list" id="customServices"></div>
  </section>

  <!-- Premium Section -->
  <section id="premiumSection">
    <h2>Premium Services</h2>
    <p style="text-align:center; font-size: 1.1rem; margin-top: 12px">Premium services will be available soon.</p>
  </section>

  <section>
    <h2>Summary</h2>
    <div class="summary" id="summaryArea">
      Please select packages or events to see summary here.
    </div>
  </section>

  <script>
    // Data sources
    const packages = [
      { id: "silver", name: "Silver", range: "50,000 - 1,00,000" },
      { id: "gold", name: "Gold", range: "1,00,000 - 3,00,000" },
      { id: "diamond", name: "Diamond", range: "3,00,000 - 5,00,000" },
      { id: "platinum", name: "Platinum", range: "5,00,000 - 8,00,000" }
    ];

    const packageServicesList = [
      "Decoration", "Catering", "Photography", "Venue", "Music", "Lighting",
      "Anchor/Artist", "DJ", "Return Gift"
    ];

    const eventTypesList = [
      "Marriage Event", "Engagement Event", "Reception", "Birthday",
      "Anniversary", "Corporate Event"
    ];

    const customServicesList = [
      "Decoration", "Catering", "Conventions", "Photography & Videography",
      "Anchors & Artists", "DJ Dance & Singing", "Guest House & Rooms",
      "Cakes & Chocolate", "Games & Entertainment", "Jewellery",
      "Invitation Cards", "Vehicles", "Return Gifts",
      "Decoration Items", "Clothing & Accessories",
      "Makeup Artists", "Mehandi Artists", "Pandith", "Garlands & Accessories"
    ];

    // State variables
    let selectedPackage = null;
    let selectedPackageServices = new Set();

    let selectedCustomEvent = null;
    let selectedCustomServices = new Set();

    // DOM refs
    const navButtons = {
      package: document.getElementById("navPackage"),
      customized: document.getElementById("navCustomized"),
      premium: document.getElementById("navPremium")
    };
    const sections = {
      package: document.getElementById("packageSection"),
      customized: document.getElementById("customizedSection"),
      premium: document.getElementById("premiumSection")
    };

    const packageButtonsDiv = document.getElementById("packageButtons");
    const packageServicesDiv = document.getElementById("packageServices");

    const customEventTypesDiv = document.getElementById("customEventTypes");
    const customServicesDiv = document.getElementById("customServices");

    const summaryArea = document.getElementById("summaryArea");

    // Switch between Package, Customized, Premium
    function switchSection(section) {
      Object.keys(sections).forEach(s => {
        sections[s].classList.remove("active");
        navButtons[s].classList.remove("selected");
      });
      sections[section].classList.add("active");
      navButtons[section].classList.add("selected");

      if (section === "premium") {
        summaryArea.textContent = "Premium services will be available soon.";
      } else {
        renderSummary();
      }
    }

    // Helper: create selectable buttons
    function createSelectableButton(text, isSelected, onClick) {
      const btn = document.createElement("button");
      btn.textContent = text;
      if (isSelected) {
        btn.classList.add("selected");
      }
      btn.onclick = onClick;
      return btn;
    }

    // Render Packages
    function renderPackages() {
      packageButtonsDiv.innerHTML = "";
      packages.forEach(pkg => {
        const btn = createSelectableButton(pkg.name + ` (${pkg.range})`, selectedPackage?.id === pkg.id, () => {
          selectedPackage = pkg;
          selectedPackageServices.clear();
          renderPackages();
          renderPackageServices();
          renderSummary();
        });
        packageButtonsDiv.appendChild(btn);
      });
    }

    // Render services for selected package
    function renderPackageServices() {
      if (!selectedPackage) {
        packageServicesDiv.innerHTML = "";
        return;
      }
      packageServicesDiv.innerHTML = "";
      packageServicesList.forEach(svc => {
        const btn = createSelectableButton(svc, selectedPackageServices.has(svc), () => {
          if (selectedPackageServices.has(svc)) {
            selectedPackageServices.delete(svc);
          } else {
            selectedPackageServices.add(svc);
          }
          renderPackageServices();
          renderSummary();
        });
        packageServicesDiv.appendChild(btn);
      });
      sections.package.scrollIntoView({ behavior: "smooth" });
    }

    // Render Event types
    function renderCustomEvents() {
      customEventTypesDiv.innerHTML = "";
      eventTypesList.forEach(evt => {
        const btn = createSelectableButton(evt, selectedCustomEvent === evt, () => {
          selectedCustomEvent = evt;
          selectedCustomServices.clear();
          renderCustomEvents();
          renderCustomServices();
          renderSummary();
        });
        customEventTypesDiv.appendChild(btn);
      });
    }

    // Render services for customized event
    function renderCustomServices() {
      if (!selectedCustomEvent) {
        customServicesDiv.innerHTML = "";
        return;
      }
      customServicesDiv.innerHTML = "";
      customServicesList.forEach(svc => {
        const btn = createSelectableButton(svc, selectedCustomServices.has(svc), () => {
          if (selectedCustomServices.has(svc)) {
            selectedCustomServices.delete(svc);
          } else {
            selectedCustomServices.add(svc);
          }
          renderCustomServices();
          renderSummary();
        });
        customServicesDiv.appendChild(btn);
      });
      sections.customized.scrollIntoView({ behavior: "smooth" });
    }

    // Render summary
    function renderSummary() {
      if (sections.premium.classList.contains("active")) return;
      let html = "<ul>";
      if (selectedPackage) {
        html += `<li><b>Selected Package:</b> ${selectedPackage.name} (${selectedPackage.range})</li>`;
        if (selectedPackageServices.size)
          html += `<li><b>Package Services:</b> ${Array.from(selectedPackageServices).join(", ")}</li>`;
      }
      if (selectedCustomEvent) {
        html += `<li><b>Selected Event:</b> ${selectedCustomEvent}</li>`;
        if (selectedCustomServices.size)
          html += `<li><b>Customized Services:</b> ${Array.from(selectedCustomServices).join(", ")}</li>`;
      }
      html += "</ul>";
      summaryArea.innerHTML = html || "No selections yet.";
    }

    // Initial render
    renderPackages();
    renderCustomEvents();
    switchSection('package');
  </script>
</body>

</html>
