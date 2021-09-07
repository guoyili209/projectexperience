Rogdoll
==
[toc]
```c#
public void ChangeRagdoll(){
    CopyCharacterTransformToRagdoll(charobj.transform,ragdollObj.transform);

    charObj.SetActive(false);
    ragdollObj.SetActive(true);

    spine.AddForce(new Vector3(0,0,-300f),ForceMode.Impulse);
}
private void CopyCharacterTransformToRagdoll(Transform origin,Transform ragdoll){
    for(int i=0;i<origin.childCount;i++){
        if(origin.childCount!=0){
            CopyCharacterTransformToRagdoll(origin.GetChild(i),ragdoll.GetChild(i));
        }
        ragdoll.GetChild(i).localPosition = origin.GetChild(i).localPosition;
        ragdoll.GetChild(i).localRotation = orgin.GetChild(i).localPosition;
    }
}
```