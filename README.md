# Birthday-for-friend
const birthdayMonth = 9; // October, zero-indexed
const birthdayDate = 29;

function getNextBirthday() {
  const now = new Date();
  let year = now.getFullYear();
  const target = new Date(year, birthdayMonth, birthdayDate, 0, 0, 0);
  if (target <= now) year += 1;
  return new Date(year, birthdayMonth, birthdayDate, 0, 0, 0);
}

const countdownTarget = getNextBirthday();
const countdownEls = {
  days: document.getElementById('days'),
  hours: document.getElementById('hours'),
  minutes: document.getElementById('minutes'),
  seconds: document.getElementById('seconds')
};

function pad(value) {
  return String(value).padStart(2, '0');
}

function updateCountdown() {
  const difference = Math.max(0, countdownTarget.getTime() - Date.now());
  const totalSeconds = Math.floor(difference / 1000);
  const days = Math.floor(totalSeconds / 86400);
  const hours = Math.floor((totalSeconds % 86400) / 3600);
  const minutes = Math.floor((totalSeconds % 3600) / 60);
  const seconds = totalSeconds % 60;

  countdownEls.days.textContent = pad(days);
  countdownEls.hours.textContent = pad(hours);
  countdownEls.minutes.textContent = pad(minutes);
  countdownEls.seconds.textContent = pad(seconds);
}

updateCountdown();
setInterval(updateCountdown, 1000);

const mapStars = [...document.querySelectorAll('.map-star')];
const noteQuote = document.querySelector('.note-display__quote');
const noteNumber = document.querySelector('.note-display__number');

mapStars.forEach((star, index) => {
  star.addEventListener('click', () => {
    mapStars.forEach((item) => item.classList.remove('is-active'));
    star.classList.add('is-active');
    noteQuote.textContent = `“${star.dataset.note}”`;
    noteNumber.textContent = `${String(index + 1).padStart(2, '0')} / 04`;
  });
});

const wishForm = document.getElementById('wishForm');
const wishInput = document.getElementById('wishInput');
const wishTags = document.getElementById('wishTags');
const toast = document.getElementById('toast');
let toastTimer;

function showToast(message) {
  toast.textContent = message;
  toast.classList.add('is-visible');
  window.clearTimeout(toastTimer);
  toastTimer = window.setTimeout(() => toast.classList.remove('is-visible'), 2600);
}

wishForm.addEventListener('submit', (event) => {
  event.preventDefault();
  const value = wishInput.value.trim();
  if (!value) {
    wishInput.focus();
    return;
  }

  const tag = document.createElement('span');
  tag.className = 'wish-tag wish-tag--cream';
  tag.innerHTML = `${escapeHtml(value)} <b>✦</b>`;
  wishTags.prepend(tag);
  wishInput.value = '';
  showToast('Wish pinned to the wall ✦');
});

function escapeHtml(value) {
  const div = document.createElement('div');
  div.textContent = value;
  return div.innerHTML;
}

const surpriseModal = document.getElementById('surpriseModal');
const openSurprise = document.getElementById('openSurprise');
const celebrateButton = document.getElementById('celebrateButton');
const modalClose = document.getElementById('modalClose');
const confettiLayer = document.getElementById('confettiLayer');

function openTheSurprise() {
  if (typeof surpriseModal.showModal === 'function') {
    surpriseModal.showModal();
  } else {
    surpriseModal.setAttribute('open', '');
  }
  burstConfetti();
}

function closeTheSurprise() {
  if (typeof surpriseModal.close === 'function') surpriseModal.close();
  else surpriseModal.removeAttribute('open');
}

openSurprise.addEventListener('click', openTheSurprise);
celebrateButton.addEventListener('click', openTheSurprise);
modalClose.addEventListener('click', closeTheSurprise);
surpriseModal.addEventListener('click', (event) => {
  if (event.target === surpriseModal) closeTheSurprise();
});

document.addEventListener('keydown', (event) => {
  if (event.key === 'Escape' && surpriseModal.open) closeTheSurprise();
});

function burstConfetti() {
  confettiLayer.replaceChildren();
  const colors = ['lime', 'blue', 'mint', 'cream'];
  for (let i = 0; i < 46; i += 1) {
    const piece = document.createElement('span');
    piece.className = 'confetti';
    piece.style.left = `${Math.random() * 100}%`;
    piece.style.setProperty('--drift', `${(Math.random() - 0.5) * 240}px`);
    piece.style.animationDelay = `${Math.random() * .45}s`;
    piece.style.transform = `rotate(${Math.random() * 180}deg)`;
    piece.classList.add(colors[i % colors.length]);
    confettiLayer.appendChild(piece);
  }
  window.setTimeout(() => confettiLayer.replaceChildren(), 3300);
}

const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (!reduceMotion) {
  const heroVisual = document.querySelector('.hero__visual');
  window.addEventListener('pointermove', (event) => {
    const x = (event.clientX / window.innerWidth - .5) * 6;
    const y = (event.clientY / window.innerHeight - .5) * 5;
    heroVisual.style.transform = `translate(${x}px, ${y}px)`;
  }, { passive: true });
}
