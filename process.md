You are an expert software agent specializing in advanced BPMN 2.0 architectures and SpiffWorkflow enterprise integrations.

I need you to upgrade our "Construction Progress Payment Approval Process" (İnşaat Hakediş Kontrol ve Onay Süreci) into a multi-stage enterprise repository. It must include multi-file upload capabilities and 2 new structural forms (HSE and Finance pre-check).

Please create/overwrite the following files with their precise content and paths inside the workspace:

1. process_metadata.json:
- Update/maintain the process id as "insaat_hakedis_sureci"
- Ensure it points to "hakedis_onay_sureci.bpmn"

2. forms/hakedis_giris.json (Stage 1: Field Engineer Form with File Upload):
- Fields: 
  * proje_adi (Enum: Kuleli Konakları, Çengelköy Rezidans, Kadıköy Ofis Park)
  * taseron_firma (string)
  * donem (string, regex pattern for MM/YYYY)
  * talep_edilen_tutar (number)
  * aciklama (textarea)
  * metraj_cetveli (type: string, widget: file or spiff-file-upload, description: "Excel formatında metraj cetveli yükleyin")
  * saha_fotograflari (type: string, widget: file or spiff-file-upload, description: "Yapılan imalatın fotoğrafları veya PDF raporu")
- All fields are required.

3. forms/hse_kontrol.json (Stage 2: New - HSE / İSG Uzmanı Formu):
- Fields:
  * isg_ihlal_var_mi (boolean, default false)
  * kesinti_tutari (number, title: "İSG İhlal Kesinti Tutarı (TL)")
  * isg_raporu (type: string, widget: file or spiff-file-upload, description: "Aylık İSG Denetim Raporu PDF")
  * hse_notlari (textarea)
- Required: isg_ihlal_var_mi, hse_notlari

4. forms/pm_onay.json (Stage 3: Project Manager Approval Form):
- Fields: pm_onay_durum (boolean, default true), pm_gerekce (textarea).
- PM must see previous fields context (read-only) and decide.

5. forms/finans_kontrol.json (Stage 4: New - Muhasebe / Finans Ön Kontrol Formu):
- Fields:
  * vergi_borcu_yoktur_belgesi (type: string, widget: file or spiff-file-upload, description: "Taşeronun güncel vergi borcu yoktur yazısı")
  * finans_onay_durum (boolean, default true)
  * finans_notlari (textarea)
- Required: vergi_borcu_yoktur_belgesi, finans_onay_durum

6. forms/gm_onay.json (Stage 5: General Manager / CFO Final Approval Form):
- Fields: gm_onay_durum (boolean, default true), gm_gerekce (textarea).

7. hakedis_onay_sureci.bpmn:
- Generate a fully valid BPMN 2.0 XML incorporating the new 5-stage sequential flow with rejections.
- The pipeline MUST be: 
  Start Event -> 
  User Task (Saha Hakediş Girişi - forms/hakedis_giris.json) -> 
  User Task (İSG Kontrolü - forms/hse_kontrol.json) -> 
  User Task (PM Onayı - forms/pm_onay.json) -> 
  Exclusive Gateway (PM Karar: if false -> End Reject) -> 
  User Task (Finans Ön Kontrol - forms/finans_kontrol.json) -> 
  Exclusive Gateway (Finans Karar: if false -> End Reject) ->
  User Task (GM Final Onayı - forms/gm_onay.json) -> 
  Exclusive Gateway (GM Karar: if true -> End Approved, if false -> End Reject)
- Injection of `<spiffworkflow:properties>` extension elements with exact form file paths for all 5 User Tasks is MANDATORY.
- Gateway routing sequences must use strict Python syntax expressions (e.g., `pm_onay_durum == True`, `finans_onay_durum == False`).
- Programmatically calculate and generate all visual DI coordinates (BPMNDiagram, Shapes, and Edges) so it opens flawlessly in the SpiffWorkflow BPMN designer.

Do not write placeholders, step-by-step lists, or snippets. Act as a live vibe coding agent: construct the directory structure and write the complete code directly into the workspace files right now.