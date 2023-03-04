
## Спроектировать базу(ы) данных для социальной сети ВКонтакте:

*   анкеты людей (имя, описание, фото, город, интересы, ...)
*   посты (описание, фото / видео / аудио, хэштеги, лайки, просмотры, комментарии ...)
*   личные сообщения и чаты (текст, прочитанность сообщений)
*   отношения (друзья, подписчики)
*   сообщества (люди, посты)
*   медиа (фото, аудио, видео)

### Postgress DB

*   анкеты людей (имя, описание, фото, город, интересы, ...)

```plaintext
profiles
	id: UUID
	small_avatar_url: string
	medium_avatar_url: string
	big_avatar_url: string
	first_name: string
	last_name: string
	about_self: text
	interests: text
	city_id: UUID
```

*   посты (описание, фото / видео / аудио, хэштеги, лайки, просмотры, комментарии ...)

```plaintext
posts
	id: UUID
	resource_id: UUID
	resource_type: user | group | community
	created_at: datetime
	description: text
	hashtags: text
	media_url: string
	preview_media_url: string
	likes: int
	reposts: int
	views: int
```

*   комментарии к постам, видео…

```plaintext
comments:
	id: UUID
	resource_id: UUID
	resource_type: photo | video | audio | post
	body: text
	author_id: UUID
	author_avatar_url: string
	parent_comment_id: UUID
	created_at: datetime
	updated_at: datetime
```

*   личные сообщения и чаты (текст, прочитанность сообщений)

```plaintext
messages
	id: UUID
	sent_at: datetime
	received_at: datetime
	read_at: datetime
	type: chat | group_chat
	chat_id: GUID
	from_id: UUID
	to_id: UUID
	body: text
	reactions: text
```

*   сообщества (люди, посты)

```plaintext
groups
	id: UUID
	type: group | community
	name: string
	description: text
```

*   медиа (фото, аудио, видео)

```plaintext
medias:
	id: UUID
	resource_id: GUID,
	resource_type: user|post|group|group_chat
	type: photo | video | audio | file
	preview_url: string
	media_url: string
```

### Graph DB (Neo4j)

*   отношения (друзья, подписчики)

```plaintext
relations
	id: UUID
	profile_id: GUID
	resource_id: GUID,
	resource_type: friend | subscriber
	created_at: datetime
```