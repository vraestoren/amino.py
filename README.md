<h1>
  <img src="https://pbs.twimg.com/profile_images/1126922506286325761/x4T2PAkG_400x400.png" width="28" style="vertical-align:middle;" />
  amino.py
</h1>

> Mobile-API for the [Amino](https://aminoapps.com/) social network providing a clean interface for authentication, messaging, communities, blogs, and more.

## Requirements

```
requests
websocket-client
json-minify
```

---

## Quick Start

```python
from amino import Amino

client = Amino()
client.login("example@gmail.com", "password")
print(client.user_id)
```

---

## Initialization

```python
Amino(device_id=None, proxies=None)
```

| Parameter   | Type   | Default | Description                                          |
|-------------|--------|---------|------------------------------------------------------|
| `device_id` | `str`  | `None`  | Custom device ID. Auto-generated if not provided.    |
| `proxies`   | `dict` | `None`  | Proxy dict passed directly to `requests.Session`.    |

---

## Authentication

### Login
```python
client.login(email, password, socket=True)
```
Logs in with email and password. Opens a WebSocket connection by default. Sets `client.sid`, `client.user_id`, and the `NDCAUTH` session header.

### Register (email)
```python
client.register(nickname, email, password, device_id, verification_code)
```

### Register (phone)
```python
client.register_phone(phone_number, nickname, password, device_id, verification_code)
```

### Activate Account
```python
client.activate_account(email, verification_code)
```

### Request Verification Code
```python
client.request_verify_code(phone_number=None, email=None, reset_password=False)
```

### Change Password
```python
client.change_password(password, new_password)
```

---

## WebSocket / Live Listening

```python
# Manually reconnect the socket
client.reload_socket()

# Block and listen for the next incoming event
event = client.listen()
print(event)
```

The socket auto-reconnects if it has been idle for more than 100 seconds.

---

## Communities

```python
# Get communities the account has joined
client.my_communities(start=0, size=25)

# Get detailed info about a community
client.get_community_info(ndc_id)

# Join / leave a community
client.join_community(ndc_id, invitation_id=None)

# Get invite codes for a community
client.get_invite_codes(ndc_id, status="normal", start=0, size=25)

# Daily check-in
client.check_in(ndc_id, tz=None)

# Spin the lottery
client.lottery(ndc_id, tz=None)
```

---

## Users

```python
client.get_user(ndc_id, user_id)
client.get_online_users(ndc_id, start=0, size=25)
client.get_recent_users(ndc_id, start=0, size=25)
client.get_user_following(ndc_id, user_id, start=0, size=25)
client.get_user_followers(ndc_id, user_id, start=0, size=25)

client.follow_user(ndc_id, user_id)
client.unfollow_user(ndc_id, user_id)
client.block_user(ndc_id, user_id)
client.unblock_user(ndc_id, user_id)

# Staff actions
client.ban_user(ndc_id, user_id, reason, ban_type=None)
client.unban_user(ndc_id, user_id, reason)
client.give_curator(ndc_id, user_id)
client.give_leader(ndc_id, user_id)

# Profile
client.edit_profile(ndc_id, nickname=None, content=None, background_color=None,
                    titles=None, colors=None, default_bubble_id=None)
client.set_activity_status(ndc_id, status)
client.comment_profile(ndc_id, content, user_id)
```

---

## Chat

```python
# Threads
client.my_chat_threads(ndc_id, start=0, size=25)
client.get_public_chat_threads(ndc_id, start=0, size=10)
client.get_chat(ndc_id, chat_id)
client.create_chat_thread(ndc_id, message, user_id)
client.search_user_chat(ndc_id, user_id)
client.edit_chat(ndc_id, chat_id, content=None, title=None, background_image=None)
client.delete_chat(ndc_id, chat_id)

# Membership
client.join_chat(ndc_id, chat_id)
client.leave_chat(ndc_id, chat_id)
client.get_chat_users(ndc_id, chat_id, start=0, size=25)
client.invite_to_chat(ndc_id, chat_id, user_id)   # user_id can be a list
client.kick_user(ndc_id, chat_id, user_id, allow_rejoin=0)
client.transfer_host(ndc_id, chat_id, user_ids)
client.accept_host(ndc_id, chat_id)

# Messages
client.get_chat_messages(ndc_id, chat_id, size=10)
client.send_message(ndc_id, chat_id, message, message_type=0,
                    reply_message_id=None, notification=None)
client.send_image(ndc_id, chat_id, image)   # path to image file
client.send_audio(path, ndc_id, chat_id)
client.send_gif(ndc_id, chat_id, gif)
client.send_embed(ndc_id, chat_id, link=None, message=None,
                  embed_title=None, embed_content=None, embed_image=None)
client.delete_message(ndc_id, chat_id, message_id, reason=None, as_staff=False)

# Voice chat
client.change_vc_permission(ndc_id, chat_id, permission)
client.invite_to_vc(ndc_id, chat_id, user_id)

# Tipping
client.send_coins_chat(ndc_id, chat_id, coins, transaction_id=None)
client.thank_tip(ndc_id, chat_id, user_id)
```

---

## Blogs

```python
client.get_blog_info(ndc_id, blog_id)
client.get_user_blogs(ndc_id, user_id, start=0, size=25)
client.get_recent_blogs(ndc_id, start=0, size=10)
client.like_blog(ndc_id, blog_id)
client.post_blog(ndc_id, title, content, image_list=None, caption_list=None,
                 categories_list=None, background_color=None, fans_only=False)
client.repost_blog(ndc_id, content=None, blog_id=None, wiki_id=None)
client.send_coins_blog(ndc_id, blog_id, coins, transaction_id=None)
client.get_tipped_users_wall(ndc_id, blog_id, start=0, size=25)
```

---

## Wallet

```python
client.get_wallet_info()
client.get_wallet_history(start=0, size=25)
client.watch_ad()   # earn coins by watching an ad
```

---

## Notifications

```python
client.get_notifications(ndc_id, start=0, size=10)
client.delete_notification(ndc_id, notification_id)
client.clear_notifications(ndc_id)
```

---

## Moderation

```python
client.moderation_history_community(ndc_id, size=25)
client.moderation_history_user(ndc_id, user_id, size=25)
client.moderation_history_blog(ndc_id, blog_id, size=25)
client.moderation_history_quiz(ndc_id, quiz_id, size=25)
client.moderation_history_wiki(ndc_id, wiki_id, size=25)
```

---

## Misc

```python
client.get_from_code(code)           # resolve an Amino short link
client.get_from_device_id(device_id) # get user from device ID
client.check_device_id(device_id)    # validate a device ID with the server
client.send_active_object(ndc_id, start_time=None, end_time=None, timers=None)
client.create_sticker_pack(ndc_id, name, stickers)
client.get_bubble_info(ndc_id, bubble_id)
client.buy_bubble(ndc_id, bubble_id)
```

---

## Notes

- All methods return the raw API response as a `dict`.
- `ndc_id` is the numeric community ID (found in Amino URLs as `x<ndc_id>`).
- The WebSocket connection auto-reconnects on errors during `listen()`.
- This is an **unofficial** wrapper. Use responsibly and in accordance with Amino's Terms of Service.

---
