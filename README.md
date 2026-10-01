# Dropout♡ v3.1

A fully customisable English school planner.

## Features

- Supabase account login and cloud sync
- Day / week / month calendar
- Date-based classes
- Optional weekly recurring classes
- Timetable
- Tasks
- Assignments
- Exams and quizzes
- Term progress
- Subject → Topic → Notes
- Note attachments: PPTX, PDF, DOCX, XLSX, CSV, TXT, ZIP, images and more
- Search and subject filters
- Planner Assistant
- Full colour customisation
- Editable labels and headers
- Custom wallpaper
- Custom stickers
- Custom app icon
- PWA support for phone, iPad and desktop

## Required Supabase Storage setup for note files

Run this once in **Supabase → SQL Editor**:

```sql
insert into storage.buckets (id, name, public)
values ('note-files', 'note-files', false)
on conflict (id) do nothing;

drop policy if exists "Users can read own note files" on storage.objects;
drop policy if exists "Users can upload own note files" on storage.objects;
drop policy if exists "Users can update own note files" on storage.objects;
drop policy if exists "Users can delete own note files" on storage.objects;

create policy "Users can read own note files"
on storage.objects
for select
to authenticated
using (
  bucket_id = 'note-files'
  and (storage.foldername(name))[1] = (select auth.uid()::text)
);

create policy "Users can upload own note files"
on storage.objects
for insert
to authenticated
with check (
  bucket_id = 'note-files'
  and (storage.foldername(name))[1] = (select auth.uid()::text)
);

create policy "Users can update own note files"
on storage.objects
for update
to authenticated
using (
  bucket_id = 'note-files'
  and (storage.foldername(name))[1] = (select auth.uid()::text)
)
with check (
  bucket_id = 'note-files'
  and (storage.foldername(name))[1] = (select auth.uid()::text)
);

create policy "Users can delete own note files"
on storage.objects
for delete
to authenticated
using (
  bucket_id = 'note-files'
  and (storage.foldername(name))[1] = (select auth.uid()::text)
);
```

The `note-files` bucket is private. The app generates short-lived signed links when you open attachments.

The existing `planner_data` table and policies are still required for planner sync.


## v3.2 Supabase project

Configured for:

- Project URL: `https://ubnqqbiwttauzlzkuscn.supabase.co`
- Publishable browser key: configured in `app.js`

You still need to create the `planner_data` table and the private `note-files` storage bucket/policies in this new Supabase project.
