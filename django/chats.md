# Чаты на Django
+ [Оглавление](../README.md)
## Модели
```python
class TrackableDateModel(models.Model):
    """Abstract model to Track the creation/updated date for a model."""

    created_on = models.DateTimeField(default=timezone.now, editable=False)
    updated_on = models.DateTimeField(default=timezone.now)

    class Meta:
        abstract = True


class ChatSession(TrackableDateModel):
    """ A Chat Session"""

    class Meta:
        ordering = ["-updated_on","-created_on","id"]


class ChatSessionMessage(TrackableDateModel):
    """Store messages for a session."""

    user = models.ForeignKey(User, on_delete=models.CASCADE)
    chat_session = models.ForeignKey(
        ChatSession, related_name='messages', on_delete=models.CASCADE
    )
    message = models.TextField(max_length=2000, blank=True, null=True, default=None)
    file = models.FileField(upload_to="files/%Y/%m/%d", max_length=255, null=True, blank=True)
    is_read = models.BooleanField(default=False)

    class Meta:
        ordering = ["-created_on", "id"]


class ChatSessionMember(TrackableDateModel):
    """Store all users in a chat session."""

    chat_session = models.ForeignKey(
        ChatSession, related_name='members', on_delete=models.CASCADE
    )
    user = models.ForeignKey(User, on_delete=models.CASCADE)

    class Meta:
        unique_together = ("chat_session", "user")
```
