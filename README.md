# RENTIT

RENTIT is a peer-to-peer rental marketplace. People list household items they rarely use (drills, tents, projectors, party gear), and others nearby send a request to borrow them for a set period.

## Architecture

```text
                ┌──────────────────────────────────┐
                │              USERS               │
                │   Owner                Renter    │
                │   (lists items)   (rents items)  │
                └────────────────┬─────────────────┘
                                 │
                                 ▼
                ┌──────────────────────────────────┐
                │        RENTIT MOBILE APP         │
                │      (Expo / React Native)       │
                │                                  │
                │  Explore · List Item · Rentals   │
                │       Inbox · Profile            │
                └────────────────┬─────────────────┘
                                 │  secure requests (login token)
                                 ▼
┌──────────────────────────────────────────────────────────────────┐
│                        SUPABASE (Backend)                        │
│                                                                  │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────────┐  │
│  │   AUTH    │  │  STORAGE  │  │ REALTIME  │  │ EDGE FUNCTIONS│  │
│  │  sign up  │  │   item    │  │   live    │  │ notifications │  │
│  │  log in   │  │  photos   │  │   chat    │  │ & rental rules│  │
│  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘  └───────┬───────┘  │
│        │              │              │                │          │
│        └──────────────┴──────┬───────┴────────────────┘          │
│                              ▼                                   │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │                   POSTGRESQL DATABASE                      │  │
│  │                                                            │  │
│  │  Users · Listings · Photos · Rentals · Messages · Reviews  │  │
│  │                                                            │  │
│  │  Security rules: each user only sees their own data        │  │
│  │  Booking rule: no two rentals of one item can overlap      │  │
│  └────────────────────────────────────────────────────────────┘  │
└────────────────────────────────┬─────────────────────────────────┘
                                 │
                                 ▼
                ┌──────────────────────────────────┐
                │       PUSH NOTIFICATIONS         │
                │   "New request!"   ──► Owner     │
                │   "Accepted!"      ──► Renter    │
                └──────────────────────────────────┘
```

## How a rental works

```text
 Renter finds item ──► Sends request ──► Owner accepts? ──No──► Declined
                                               │
                                              Yes
                                               ▼
     Both leave reviews ◄── Item returned ◄── Pickup ◄── Chat to arrange pickup
```
