const demoUsers = {
  superadmin: {
    username: 'superadmin',
    password: 'admin123',
    role: 'Super Admin',
    greeting: 'Good afternoon, Super Admin.',
    allowedControls: true,
    allowedUsers: true,
  },
  admin: {
    username: 'admin',
    password: 'admin123',
    role: 'Admin',
    greeting: 'Good afternoon, Admin.',
    allowedControls: true,
    allowedUsers: false,
  },
  user: {
    username: 'user',
    password: 'user123',
    role: 'User',
    greeting: 'Good afternoon, CCS User.',
    allowedControls: false,
    allowedUsers: false,
  },
};

const loginScreen = document.getElementById('loginScreen');
const dashboardScreen = document.getElementById('dashboardScreen');
const loginForm = document.getElementById('loginForm');
const loginError = document.getElementById('loginError');
const usernameInput = document.getElementById('username');
const passwordInput = document.getElementById('password');
const welcomeMessage = document.getElementById('welcomeMessage');
const adminControls = document.getElementById('adminControls');
const superAdminPanel = document.getElementById('superAdminPanel');
const logoutBtn = document.getElementById('logoutBtn');
const demoButtons = document.querySelectorAll('.demo-btn');

function setDemoAccount(username) {
  usernameInput.value = username;
  const passwordMap = {
    superadmin: 'admin123',
    admin: 'admin123',
    user: 'user123',
  };
  passwordInput.value = passwordMap[username] || '';

  demoButtons.forEach((btn) => {
    btn.classList.toggle('active', btn.dataset.user === username);
  });
}

demoButtons.forEach((button) => {
  button.addEventListener('click', () => setDemoAccount(button.dataset.user));
});

function renderRoleAccess(user) {
  const isSuper = user.role === 'Super Admin';
  const isAdmin = user.role === 'Admin';

  adminControls.classList.toggle('hidden', !(isAdmin || isSuper));
  superAdminPanel.classList.toggle('hidden', !isSuper);

  if (user.role === 'User') {
    welcomeMessage.textContent = 'Good afternoon, CCS User.';
  } else {
    welcomeMessage.textContent = user.greeting;
  }
}

function handleLogin(event) {
  event.preventDefault();
  const username = usernameInput.value.trim();
  const password = passwordInput.value.trim();
  const user = demoUsers[username];

  if (!user || user.password !== password) {
    loginError.textContent = 'Invalid username or password.';
    return;
  }

  loginError.textContent = '';
  renderRoleAccess(user);
  loginScreen.classList.remove('active');
  dashboardScreen.classList.add('active');
}

function handleLogout() {
  dashboardScreen.classList.remove('active');
  loginScreen.classList.add('active');
  loginForm.reset();
  setDemoAccount('superadmin');
  loginError.textContent = '';
}

loginForm.addEventListener('submit', handleLogin);
logoutBtn.addEventListener('click', handleLogout);

setDemoAccount('superadmin');
renderRoleAccess(demoUsers.superadmin);
