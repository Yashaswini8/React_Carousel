# Ex05 Image Carousel
## Date:24-05-2026
## Reg No:212224220123

## AIM
To create a Image Carousel using React 

## ALGORITHM
### STEP 1 Initial Setup:
Input: A list of images to display in the carousel.

Output: A component displaying the images with navigation controls (e.g., next/previous buttons).

### Step 2 State Management:
Use a state variable (currentIndex) to track the index of the current image displayed.

The carousel starts with the first image, so initialize currentIndex to 0.

### Step 3 Navigation Controls:
Next Image: When the "Next" button is clicked, increment currentIndex.

If currentIndex is at the end of the image list (last image), loop back to the first image using modulo:
currentIndex = (currentIndex + 1) % images.length;

Previous Image: When the "Previous" button is clicked, decrement currentIndex.

If currentIndex is at the beginning (first image), loop back to the last image:
currentIndex = (currentIndex - 1 + images.length) % images.length;

### Step 4 Displaying the Image:
The currentIndex determines which image is displayed.

Using the currentIndex, display the corresponding image from the images list.

### Step 5 Auto-Rotation:
Set an interval to automatically change the image after a set amount of time (e.g., 3 seconds).

Use setInterval to call the nextImage() function at regular intervals.

Clean up the interval when the component unmounts using clearInterval to prevent memory leaks.

## PROGRAM
## Carousel.js:

```
import React, { useState, useEffect } from 'react';
import './Carousel.css';

// Using premium creative art, sketching, and digital design concept images
const CAROUSEL_IMAGES = [
    {
        url: "https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?auto=format&fit=crop&w=1000&q=80",
        title: "Digital Canvas & Core Aesthetics",
        caption: "Where modern software vectors merge seamlessly with traditional oil paint textures."
    },
    {
        url: "https://images.unsplash.com/photo-1513364776144-60967b0f800f?auto=format&fit=crop&w=1000&q=80",
        title: "Traditional Brush & Beyond",
        caption: "Exploring vibrant acrylic dimensions, fine line sketching, and textured paper design."
    },
    {
        url: "https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?auto=format&fit=crop&w=1000&q=80",
        title: "Abstract Geometric Minimalism",
        caption: "Unlocking structural depth through clean compositions, curves, and pastel gradients."
    }
];

const Carousel = () => {
    const [currentIndex, setCurrentIndex] = useState(0);
    const [isPlaying, setIsPlaying] = useState(true);

    const nextImage = () => {
        setCurrentIndex((prevIndex) => (prevIndex + 1) % CAROUSEL_IMAGES.length);
    };

    const prevImage = () => {
        setCurrentIndex((prevIndex) => (prevIndex - 1 + CAROUSEL_IMAGES.length) % CAROUSEL_IMAGES.length);
    };

    useEffect(() => {
        let rotationInterval = null;

        if (isPlaying) {
            rotationInterval = setInterval(() => {
                nextImage();
            }, 3000);
        }

        return () => {
            if (rotationInterval) {
                clearInterval(rotationInterval);
            }
        };
    }, [isPlaying, currentIndex]);

    return (
        <div className="carousel-wrapper">
            <div className="carousel-container">
                
                {/* Brand Header Display Area */}
                <div className="carousel-header">
                    <span className="brand-label">Yashaswi <span>Creative Studio</span></span>
                    <button 
                        className={`play-pause-btn ${!isPlaying ? 'paused' : ''}`}
                        onClick={() => setIsPlaying(!isPlaying)}
                    >
                        {isPlaying ? "● AUTOPLAY ON" : "○ PAUSED"}
                    </button>
                </div>

                {/* Main Viewport Slider Deck */}
                <div className="carousel-viewport">
                    {CAROUSEL_IMAGES.map((image, index) => {
                        let positionClass = "slide next-slide";
                        if (index === currentIndex) {
                            positionClass = "slide active-slide";
                        } else if (index === (currentIndex - 1 + CAROUSEL_IMAGES.length) % CAROUSEL_IMAGES.length) {
                            positionClass = "slide prev-slide";
                        }

                        return (
                            <div className={positionClass} key={index}>
                                <img src={image.url} alt={image.title} className="slide-image" />
                                <div className="slide-card-overlay">
                                    <h3>{image.title}</h3>
                                    <p>{image.caption}</p>
                                </div>
                            </div>
                        );
                    })}

                    {/* Navigation Handle Controls */}
                    <button onClick={prevImage} className="arrow-control left-arrow" aria-label="Previous Slide">&#x276E;</button>
                    <button onClick={nextImage} className="arrow-control right-arrow" aria-label="Next Slide">&#x276F;</button>
                </div>

                {/* Progress Dot Indicators */}
                <div className="carousel-dots">
                    {CAROUSEL_IMAGES.map((_, index) => (
                        <span 
                            key={index} 
                            className={`dot ${index === currentIndex ? 'active-dot' : ''}`}
                            onClick={() => setCurrentIndex(index)}
                        ></span>
                    ))}
                </div>
            </div>
        </div>
    );
};

export default Carousel;
```
## Caroousel.css:
```
/* Centered Layout Wrapper Layout Background */
.carousel-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: radial-gradient(circle at center, #1e293b 0%, #0f172a 100%);
    padding: 20px;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

/* Master Component Case Deck */
.carousel-container {
    background: #111827;
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 24px;
    padding: 24px;
    width: 100%;
    max-width: 750px;
    box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
}

/* Header Identification Section */
.carousel-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 18px;
    padding: 0 4px;
}

.brand-label {
    font-size: 0.85rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: #9ca3af;
}

/* Find these specific classes in your Carousel.css and update their color rules: */

.brand-label span {
    color: #8b5cf6; /* Artistic violet accent */
    font-weight: 400;
}

.arrow-control:hover {
    background: #8b5cf6; /* Artistic violet arrow glow */
    color: #ffffff;
    box-shadow: 0 0 12px rgba(139, 92, 246, 0.4);
}

.dot.active-dot {
    background: #8b5cf6; /* Active tracking bar match */
    width: 24px;
    border-radius: 4px;
    box-shadow: 0 0 8px rgba(139, 92, 246, 0.5);
}

.play-pause-btn {
    background: rgba(16, 185, 129, 0.1);
    border: none;
    outline: none;
    color: #10b981;
    font-size: 0.75rem;
    font-weight: 600;
    padding: 6px 12px;
    border-radius: 20px;
    cursor: pointer;
    transition: all 0.2s ease;
}

.play-pause-btn.paused {
    background: rgba(239, 68, 68, 0.1);
    color: #f87171;
}

/* Framed Interactive Viewport Stage */
.carousel-viewport {
    position: relative;
    height: 400px;
    width: 100%;
    border-radius: 16px;
    overflow: hidden;
    background: #030712;
    border: 1px solid rgba(255, 255, 255, 0.03);
}

/* Sliding Component Core Animation States */
.slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.6s ease-in-out;
    display: flex;
    align-items: center;
    justify-content: center;
}

.slide-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    pointer-events: none;
}

/* Active Class Modifiers */
.active-slide {
    opacity: 1;
    transform: scale(1);
    z-index: 10;
}

.next-slide {
    transform: scale(0.95);
}

.prev-slide {
    transform: scale(0.95);
}

/* Bottom Text Floating Banner Cards */
.slide-card-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background: linear-gradient(transparent, rgba(3, 7, 18, 0.95) 85%);
    padding: 40px 30px 25px 30px;
    color: #ffffff;
    z-index: 20;
    text-shadow: 0 2px 4px rgba(0,0,0,0.4);
}

.slide-card-overlay h3 {
    font-size: 1.4rem;
    font-weight: 600;
    margin-bottom: 6px;
    color: #f3f4f6;
}

.slide-card-overlay p {
    font-size: 0.95rem;
    color: #9ca3af;
    line-height: 1.4;
}

/* Universal Action Toggle Arrows */
.arrow-control {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(17, 24, 39, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.05);
    color: #f3f4f6;
    font-size: 1.2rem;
    width: 44px;
    height: 44px;
    border-radius: 50%;
    cursor: pointer;
    z-index: 30;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all 0.2s ease;
    backdrop-filter: blur(4px);
}

.arrow-control:hover {
    background: #3b82f6;
    color: #ffffff;
}

.left-arrow { left: 16px; }
.right-arrow { right: 16px; }

/* Progress Pagination Dots Group */
.carousel-dots {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-top: 18px;
}

.dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #374151;
    cursor: pointer;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.dot:hover {
    background: #4b5563;
}

.dot.active-dot {
    background: #3b82f6;
    width: 24px;
    border-radius: 4px;
    box-shadow: 0 0 8px rgba(59, 130, 246, 0.5);
}

/* Mobile View Adjustments */
@media (max-width: 600px) {
    .carousel-container { padding: 16px; }
    .carousel-viewport { height: 280px; }
    .slide-card-overlay { padding: 20px 15px; }
    .slide-card-overlay h3 { font-size: 1.1rem; }
    .slide-card-overlay p { font-size: 0.8rem; }
    .arrow-control { width: 36px; height: 36px; font-size: 1rem; }
}

```
## App.js:
```
import React from 'react';
import Carousel from './Carousel';

function App() {
  return (
    <div className="App">
      <Carousel />
    </div>
  );
}

export default App;
```
## OUTPUT
<img width="1919" height="1088" alt="image" src="https://github.com/user-attachments/assets/f699bfc4-b4d6-4c4f-bbec-9173902c286f" />
<img width="1897" height="1094" alt="image" src="https://github.com/user-attachments/assets/2b2e1725-d73a-4adf-9219-a0ae9397e235" />
<img width="1909" height="1082" alt="image" src="https://github.com/user-attachments/assets/68dd7f2a-5ac0-4227-8280-3ea80373e7e6" />

## RESULT
The program for creating Image Carousel using React is executed successfully.
