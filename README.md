# Notes.API

### API for app: https://play.google.com/store/apps/details?id=huynguyen.hnote

####Grand permission:
>
> ######Read saved hash
>        String note_auth = getSharedPreferences(Globals.PREFS_NAME, 0).getString("_note_auth", "");
>       if("".equals(note_auth)) {
>               try{
>               Intent intent = new Intent();
>               intent.setComponent(new ComponentName("huynguyen.hnote", "huynguyen.hnote.Activity.GrantPermission"));
>               startActivityForResult(intent, DEF_REQUEST_PERRMISSION);
>               }catch(Exception ignore){//app not instaled}
>       }else {
>           testAccess(note_auth);
>       }
> ######Intent result handle
>      @Override
>       protected void onActivityResult(int requestCode, int resultCode, Intent data) {
>           super.onActivityResult(requestCode, resultCode, data);
>         if(requestCode == DEF_REQUEST_PERRMISSION){
>               try {
>                   String hash = data.getStringExtra("_hash");
>                   getSharedPreferences(Globals.PREFS_NAME, 0).edit().putString("_note_auth", hash).apply();
>                   testAccess(hash);
>               }catch (Exception ignore){}
>           }
>       }

####Access note data:
        
>       void testAccess(String hash){
>         Uri contentUri = Uri.parse("content://huynguyen.hnote.db/?hash=" + hash);
>         Uri type =  Uri.parse(getContentResolver().getType(contentUri));
>         if(type.getQueryParameter("type").equals("0")){
>               try{
>               Intent intent = new Intent();
>               intent.setComponent(new ComponentName("huynguyen.hnote", "huynguyen.hnote.Activity.GrantPermission"));
>               startActivityForResult(intent, DEF_REQUEST_PERRMISSION);
>               }catch(Exception ignore){//app not instaled}
>         }
>        Cursor cursor = getContentResolver().query(contentUri,null,null,null,null);
>         if (cursor != null && cursor.moveToFirst()) {
>             Log.w(getPackageName(),cursor.getString(0));
>         }
>       }
