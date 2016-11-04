# Notes.API

### API for app: https://play.google.com/store/apps/details?id=huynguyen.hnote

####Grand permission:
       
>        String note_auth = getSharedPreferences(Globals.PREFS_NAME, 0).getString("_note_auth", "");
>       if("".equals(note_auth)) {
>           startActivityForResult(new Intent(this, GrandPerrmison.class), DEF_REQUEST_PERRMISSION);
>       }else {
>           testAccess(note_auth);
>       }
      
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
>             startActivityForResult(new Intent(this, GrandPerrmison.class), DEF_REQUEST_PERRMISSION);
>         }
>        Cursor cursor = getContentResolver().query(contentUri,null,null,null,null);
>         if (cursor != null && cursor.moveToFirst()) {
>             Log.w(getPackageName(),cursor.getString(0));
>         }
>       }
