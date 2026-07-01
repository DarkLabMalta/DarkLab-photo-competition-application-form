<style>
  .dl-submit-wrap {
    font-family: 'Helvetica Neue', Arial, sans-serif;
    background: #202020;
    color: #f0f0f0;
  }

  .dl-submit-wrap * { box-sizing: border-box; }

  .dl-submit-header {
    background: #c42727;
    color: #ffffff;
    padding: 2.5rem 1.5rem 2rem;
    text-align: center;
  }

  .dl-submit-header h1 {
    margin: 0 0 0.4rem;
    font-size: 1.9rem;
    letter-spacing: 0.03em;
  }

  .dl-submit-header p {
    margin: 0;
    opacity: 0.9;
    font-size: 0.95rem;
  }

  .dl-submit-form {
    max-width: 560px;
    margin: 2rem auto 3rem;
    padding: 0 1.5rem;
  }

  .dl-field {
    margin-bottom: 1.2rem;
  }

  .dl-field label {
    display: block;
    font-size: 0.85rem;
    margin-bottom: 0.4rem;
    color: #d0d0d0;
  }

  .dl-field .dl-optional {
    color: #888888;
    font-weight: normal;
  }

  .dl-field input,
  .dl-field select {
    width: 100%;
    padding: 0.7rem 0.8rem;
    border: 1px solid #4a4a4a;
    border-radius: 4px;
    font-size: 0.95rem;
    background: #2b2b2b;
    color: #f0f0f0;
  }

  .dl-field input::placeholder {
    color: #888888;
  }

  .dl-field small {
    display: block;
    margin-top: 0.35rem;
    font-size: 0.78rem;
    color: #888888;
  }

  .dl-submit-button {
    width: 100%;
    background: #c42727;
    color: #ffffff;
    border: none;
    border-radius: 4px;
    padding: 0.85rem 1.1rem;
    font-size: 1rem;
    cursor: pointer;
    transition: background 0.15s ease;
  }

  .dl-submit-button:hover:not(:disabled) {
    background: #9e1f1f;
  }

  .dl-submit-button:disabled {
    background: #5a5a5a;
    cursor: default;
  }

  .dl-form-message {
    margin-top: 1rem;
    padding: 0.8rem 1rem;
    border-radius: 4px;
    font-size: 0.9rem;
    display: none;
  }

  .dl-form-message.dl-success {
    display: block;
    background: #1f3a1f;
    color: #b6e6b6;
  }

  .dl-form-message.dl-error {
    display: block;
    background: #3a1f1f;
    color: #e6b6b6;
  }
</style>

<div class="dl-submit-wrap">

  <div class="dl-submit-header">
    <h1>Submit your photo</h1>
    <p>Enter this month's competition with your best frame developed at DarkLab</p>
  </div>

  <form class="dl-submit-form" id="dl-submit-form">

    <div class="dl-field">
      <label for="dl-name">Full name</label>
      <input type="text" id="dl-name" name="name" required>
    </div>

    <div class="dl-field">
      <label for="dl-email">Email address</label>
      <input type="email" id="dl-email" name="email" required>
    </div>

    <div class="dl-field">
      <label for="dl-month">Month developed</label>
      <select id="dl-month" name="month_developed" required>
        <option value="" disabled selected>Select the month</option>
        <option value="January">January</option>
        <option value="February">February</option>
        <option value="March">March</option>
        <option value="April">April</option>
        <option value="May">May</option>
        <option value="June">June</option>
        <option value="July">July</option>
        <option value="August">August</option>
        <option value="September">September</option>
        <option value="October">October</option>
        <option value="November">November</option>
        <option value="December">December</option>
      </select>
    </div>

    <div class="dl-field">
      <label for="dl-title">Photo title <span class="dl-optional">(optional)</span></label>
      <input type="text" id="dl-title" name="title" placeholder="Leave blank if you would rather not title it">
    </div>

    <div class="dl-field">
      <label for="dl-link">Link to your photo</label>
      <input type="url" id="dl-link" name="transfer_link" placeholder="Paste your WeTransfer, Google Drive or Dropbox link" required>
      <small>Please make sure the link is set to allow anyone with the link to view it, otherwise we will not be able to open your photo.</small>
    </div>

    <input type="hidden" name="access_key" value="65f78bb7-f1ec-45e2-825f-9544e599b3bf">
    <input type="hidden" name="subject" value="New DarkLab Photo of the Month entry">
    <input type="checkbox" name="botcheck" style="display:none;">

    <button type="submit" class="dl-submit-button" id="dl-submit-button">Send entry</button>

    <div class="dl-form-message" id="dl-form-message"></div>

  </form>

</div>

<script>
(function() {
  var form = document.getElementById("dl-submit-form");
  var button = document.getElementById("dl-submit-button");
  var messageBox = document.getElementById("dl-form-message");

  form.addEventListener("submit", function(event) {
    event.preventDefault();

    button.disabled = true;
    button.textContent = "Sending...";
    messageBox.className = "dl-form-message";
    messageBox.textContent = "";

    var formData = new FormData(form);

    fetch("https://api.web3forms.com/submit", {
      method: "POST",
      headers: { "Accept": "application/json" },
      body: formData
    })
    .then(function(response) { return response.json(); })
    .then(function(result) {
      if (result.success) {
        messageBox.className = "dl-form-message dl-success";
        messageBox.textContent = "Thank you, your entry has been sent. Good luck!";
        form.reset();
        button.textContent = "Send entry";
        button.disabled = false;
      } else {
        throw new Error(result.message || "Something went wrong");
      }
    })
    .catch(function() {
      messageBox.className = "dl-form-message dl-error";
      messageBox.textContent = "Something went wrong sending your entry, please try again or email us directly.";
      button.textContent = "Send entry";
      button.disabled = false;
    });
  });
})();
</script>
