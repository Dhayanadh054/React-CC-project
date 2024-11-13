import React, { useState, useEffect } from 'react';
import axios from 'axios';
import './App.css';

const resorts = [
  { id: 1, name: 'Sunny Beach Resort', location: 'Maldives', price: '$300/Day', image: 'https://i.pinimg.com/736x/6c/44/c9/6c44c96b293f0f4577c1698cf8399b20.jpg' },
  { id: 2, name: 'Mountain View Resort', location: 'Switzerland', price: '$400/Day', image: 'https://sugbo.ph/wp-content/uploads/2017/08/1new.jpg' },
  { id: 3, name: 'Desert Oasis Resort', location: 'Dubai', price: '$350/Day', image: 'https://www.hyattvacationclub.com/files/live/sites/hvc-marketing/files/properties/desert_oasis/consolidated/DesertOasis-Exterior-pool_hrcPSPSHpo-199783_540x480_16_9.webp' },
  { id: 4, name: 'Forest Retreat', location: 'Canada', price: '$250/Day', image: 'https://homeadore.com/wp-content/uploads/2013/06/004-forest-retreat-pad-studio.jpg' },
  { id: 5, name: 'Island Paradise Resort', location: 'Bora Bora', price: '$450/Day', image: 'https://cf.bstatic.com/xdata/images/hotel/max1024x768/426439788.jpg?k=153e49342f06e83db5c995f1301a854aa1b9498db37c6560087954a8e42214ed&o=&hp=1' },
];

function App() {
  const [dateError, setDateError] = useState('');
  const [showPlainDiv, setShowPlainDiv] = useState(false);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [searchTerm, setSearchTerm] = useState('');
  const [bookingDetails, setBookingDetails] = useState(null);
  const [showModal, setShowModal] = useState(false);
  const [date, setDate] = useState('');
  const [rooms, setRooms] = useState(1);
  const [selectedResort, setSelectedResort] = useState(null);
  const [users, setUsers] = useState([]);
  const [isSignUp, setIsSignUp] = useState(false);

  useEffect(() => {
    fetchUsers();
  }, []);

  const togglePlainDiv = () => {
    setShowPlainDiv(!showPlainDiv);
  };

  const fetchUsers = async () => {
    try {
      const response = await axios.get('http://localhost:3001/users');
      setUsers(response.data);
    } catch (error) {
      console.error('Error fetching users:', error);
    }
  };

  const handleLogin = (e) => {
    e.preventDefault();
    const user = users.find(u => u.email === email && u.password === password);
    if (user) {
      setIsLoggedIn(true);
    } else {
      alert('Invalid email or password');
    }
  };

  const handleLogout = () => {
    setIsLoggedIn(false);
    setEmail('');
    setPassword('');
    setBookingDetails(null);
  };

  const handleSignUp = async (e) => {
    e.preventDefault();
    const userExists = users.find(u => u.email === email);
    if (userExists) {
      alert('User already exists');
      return;
    }
    const newUser = { email, password };
    try {
      await axios.post('http://localhost:3001/users', newUser);
      setEmail('');
      setPassword('');
      alert('Registration successful');
      setIsSignUp(false);
      fetchUsers();
    } catch (error) {
      console.error('Error registering user:', error);
    }
  };

  const filteredResorts = resorts.filter(resort =>
    resort.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  const handleBookNow = (resort) => {
    setSelectedResort(resort);
    setShowModal(true);
  };

  const handleConfirmBooking = () => {
    if (!date) { // Check if date is empty
      setDateError('Please select a date.');
      return;
    }
    const selectedDate = new Date(date);
    const today = new Date();

    // Set the time of today to 00:00:00 for accurate comparison
    today.setHours(0, 0, 0, 0);

    if (selectedDate < today) {
      setDateError('The selected date cannot be in the past.');
      return;
    }

    // Reset error message if the date is valid
    setDateError('');
    if (selectedResort) {
      const currentDate = new Date();
      const formattedDate = currentDate.toLocaleDateString();
      const formattedTime = currentDate.toLocaleTimeString();

      const newBookingDetails = {
        name: selectedResort.name,
        location: selectedResort.location,
        price: selectedResort.price,
        bookingDate: date,
        numberOfRooms: rooms,
        confirmationDate: formattedDate,
        confirmationTime: formattedTime,
      };

      setBookingDetails(newBookingDetails);
      setShowModal(false);
    }
  };

  return (
    <div className="app">
      {!isLoggedIn ? (
        <div className="login-page">
          <div className="login-container">
            <h2>BookMyStay</h2>
            {isSignUp ? (
              <form onSubmit={handleSignUp}>
                <h3>Sign Up</h3>
                <div className="input-group">
                  <label>Email</label>
                  <input
                    type="email"
                    value={email}
                    onChange={(e) => setEmail(e.target.value)}
                    required
                  />
                </div>
                <div className="input-group">
                  <label>Password</label>
                  <input
                    type="password"
                    value={password}
                    onChange={(e) => setPassword(e.target.value)}
                    required
                  />
                </div>
                <button type="submit" className="sign-up-button">Sign Up</button>
                <p>Already have an account? <span onClick={() => setIsSignUp(false)} className="toggle">Sign In</span></p>
              </form>
            ) : (
              <form onSubmit={handleLogin}>
                <div className="input-group">
                  <label>Email</label>
                  <input
                    type="email"
                    value={email}
                    onChange={(e) => setEmail(e.target.value)}
                    required
                  />
                </div>
                <div className="input-group">
                  <label>Password</label>
                  <input
                    type="password"
                    value={password}
                    onChange={(e) => setPassword(e.target.value)}
                    required
                  />
                </div>
                <button type="submit" className="login-button">Login</button>
                <div className="options">
                  <button onClick={() => alert('Forgot Password functionality will be implemented.')} className="forgot-password">
                    Forgot Password?
                  </button>
                  <p>New Here? <span onClick={() => setIsSignUp(true)} className="toggle">Sign Up</span></p>
                </div>
              </form>
            )}
          </div>
        </div>
      ) : (
        <div className="portal">
          
          <header>
            <div className="logo">
              <span>Bookie.Com</span>
            </div>
            <h2 className="portal-title">𝔀𝓮𝓵𝓬𝓸𝓶𝓮 𝓽𝓸 𝓑𝓸𝓸𝓴𝓜𝔂𝓢𝓽𝓪𝔂</h2>
            <button onClick={handleLogout} className="logout-button">Logout</button>
          </header>
          <p className="welcome-text">✨ Welcome to your dream getaway! ✨</p>
             {/* Conditionally render the plain div styled as a card just below the Book Now button */}
          {showPlainDiv && (
            <div className="card small-card">
              {bookingDetails && (
                <div className="booking-confirmation">
                  <h3>Booking Confirmed!</h3>
                  <p><strong>Resort:</strong> {bookingDetails.name}</p>
                  <p><strong>Location:</strong> {bookingDetails.location}</p>
                  <p><strong>Price:</strong> {bookingDetails.price}</p>
                  <p><strong>Booking Date:</strong> {bookingDetails.bookingDate}</p>
                  <p><strong>Number of Rooms:</strong> {bookingDetails.numberOfRooms}</p>
                  <p><strong>Confirmation Date:</strong> {bookingDetails.confirmationDate}</p>
                  <p><strong>Confirmation Time:</strong> {bookingDetails.confirmationTime}</p>
                </div>
              )}
            </div>
          )}
          <div className="search-bar">
            <input
              type="text"
              placeholder="Search resorts..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
            />
          </div>

          <div className="resorts-section">
            <div className="resorts-list">
              {filteredResorts.map(resort => (
                <div key={resort.id} className="resort-card">
                  <img src={resort.image} alt={resort.name} className="resort-image" />
                  <h3>{resort.name}</h3>
                  <p>{resort.location}</p>
                  <p className="highlighted-price">{resort.price}</p>
                  <button onClick={() => {
                    handleBookNow(resort);
                    togglePlainDiv(); // Show the booking confirmation on click
                  }} className="book-now-button">Book Now</button>
                </div>
              ))}
            </div>
          </div>

          {showModal && (
        <div className="modal">
          <div className="modal-content">
            <span className="close-button" onClick={() => setShowModal(false)}>&times;</span>
            <h3>Booking Details</h3>
            <div className="input-group">
              <label>Date</label>
              <input
                type="date"
                value={date}
                onChange={(e) => {
                  setDate(e.target.value);
                  setDateError(''); // Clear error when user changes date
                }}
                required
              />
              {dateError && <p className="error-message">{dateError}</p>} {/* Display error message */}
            </div>
            <div className="input-group">
              <label>Number of Rooms</label>
              <input
                type="number"
                value={rooms}
                onChange={(e) => setRooms(e.target.value)}
                min="1"
                required
              />
            </div>
            <button onClick={handleConfirmBooking} className="confirm-booking-button">Confirm Booking</button>
          </div>
        </div>
      )}
        </div>
      )}
    </div>
  );
}

export default App;
