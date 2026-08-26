# Generate videos with Microsoft Foundry

Source: [Generate videos with Microsoft Foundry][src-msft-docs]

## Generate video from a prompt

**Video generation parameters**:

Before crafting your prompt, understand the API parameters that control your video output:

| Parameter | Description | Supported values |
| --- | --- | --- |
| prompt | Natural language description of your video | Text string (required) |
| model | The model to use | sora-2 or sora-2-pro |
| size | Output resolution | 1280x720 (landscape), 720x1280 (portrait) |
| seconds | Video duration | 4, 8, or 12 (default: 4) |
| input_reference | Reference image for the first frame | JPEG, PNG, or WebP file |
| remix_video_id | ID of a previous video to remix | Video ID string |

## Generate video in Python

**Generate a video**:

Video generation follows a three-step pattern: create the job, poll for completion, and download the result.

```python
import time

# Create the video generation job
video = client.videos.create(
    model="sora-2",
    prompt="A robot walks through a rainy city street at dusk, neon signs reflecting in puddles",
    size="1280x720",
    seconds="4",
)

print(f"Video creation started. ID: {video.id}")

# Poll for completion
while video.status not in ["completed", "failed", "cancelled"]:
    print(f"Status: {video.status}. Waiting...")
    time.sleep(20)
    video = client.videos.retrieve(video.id)

# Download when complete
if video.status == "completed":
    content = client.videos.download_content(video.id, variant="video")
    content.write_to_file("output.mp4")
    print("Video saved to output.mp4")
```

**Generate video from a reference image**:

To use an image as a starting frame, pass it to the input_reference parameter. The image resolution must match the target video size:

```python
video = client.videos.create(
    model="sora-2",
    prompt="The camera slowly pans across the landscape as clouds drift overhead",
    size="1280x720",
    seconds="4",
    input_reference=open("landscape.png", "rb"),
)
```

**Remix an existing video**:

To modify an existing video while preserving its structure, use the remix method with the original video's ID:

```python
video = client.videos.remix(
    video_id="video_abc123",
    prompt="Change the color palette to warm sunset tones",
)
```

**Handle job states**:

Video jobs can return these status values:

| Status | Description |
| --- | --- |
| queued | Job is waiting to be processed |
| in_progress | Video is being generated |
| completed | Video is ready for download |
| failed | Generation failed (check error details) |
| cancelled | Job was canceled |

When a job fails, check `video.error` for details about what went wrong.

**Key considerations**:

- Rate limits: You can run up to two video creation jobs simultaneously.
- Job expiration: Completed videos are available for download for 24 hours.
- Resolution requirements: Reference images must match the target video resolution exactly.
- Content filtering: Prompts are subject to content moderation; harmful content won't generate.

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/generate-video-with-foundry/
