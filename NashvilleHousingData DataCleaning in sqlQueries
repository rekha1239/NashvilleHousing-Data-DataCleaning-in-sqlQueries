 --Cleaning Data in SQL queries


 select * from NashvilleHousing

 -------------------------------------------------------------------
 
 --Standardize Date Format
 

 select saleDate from NashvilleHousing

 alter table NashvilleHousing
  add  SaleDateConverted date

  update NashvilleHousing
  set SaleDateConverted = convert(date,SaleDate)

-----------------------------------------------------------------------------------
 
--Populate propertyAddress Data


 select a.PropertyAddress,a.ParcelID ,b.ParcelID,b.PropertyAddress,
 isnull(a.PropertyAddress,b.PropertyAddress) 
 as newadd
 from NashvilleHousing a
 join 
 NashvilleHousing b
 on a.ParcelID = b.ParcelID
 and a.[UniqueID ] <> b.[UniqueID ]
 where a.PropertyAddress is null
  
  update a
  set a.PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
  from NashvilleHousing a
  join 
 NashvilleHousing b
 on a.ParcelID = b.ParcelID
 and a.[UniqueID ] <> b.[UniqueID ]
 where a.PropertyAddress is null

 select * from NashvilleHousing where PropertyAddress is null

 -----------------------------------------------------------------------------------------------------

--Breaking out address into individual column(address,city,state,)


 select 
 substring(propertyAddress,1,charindex(',',PropertyAddress)-1) as propertyAddress_splitStreetAddress,
 substring(propertyAddress,charindex( ',',PropertyAddress)+1,LEN(PropertyAddress))as propertyAddress_splitCity
  from NashvilleHousing

  alter table NashvilleHousing
  add propertyAddress_splitStreetAddress nvarchar(255)

  update NashvilleHousing
  set propertyAddress_splitStreetAddress = substring(propertyAddress,1,charindex(',',PropertyAddress)-1) 
  
   alter table NashvilleHousing
  add  propertyAddress_splitCity nvarchar(255)

  update NashvilleHousing
  set propertyAddress_splitCity = substring(propertyAddress,charindex( ',',PropertyAddress)+1,LEN(PropertyAddress))

 select PARSENAME(replace(OwnerAddress,',', '.'),1) OwnerAddress_State,
       PARSENAME(replace(OwnerAddress,',', '.'),2) OwnerAddress_City,
        PARSENAME(replace(OwnerAddress,',', '.'),3) OwnerAddress_StreetAddress
 from NashvilleHousing
   
   alter table NashvilleHousing
   add OwnerAddress_State nvarchar(255),
   OwnerAddress_City  nvarchar(255),
   OwnerAddress_StreetAddress nvarchar(255)

   update NashvilleHousing
   set OwnerAddress_State = PARSENAME(replace(OwnerAddress,',', '.'),1),
       OwnerAddress_City = PARSENAME(replace(OwnerAddress,',', '.'),2),
       OwnerAddress_StreetAddress = PARSENAME(replace(OwnerAddress,',', '.'),3)

--------------------------------------------------------------------------------------------------

--change Y and N to Yes and No in "sold as vacant" field


select distinct(SoldAsVacant),count(SoldAsVacant)  from NashvilleHousing
group by SoldAsVacant 
order by 2 desc

select SoldAsVacant ,
case 
   
	 when SoldAsVacant = 'N' then 'No'
	 when SoldAsVacant = 'Y' then 'Yes'
	 else SoldAsVacant
end 
from NashvilleHousing

update NashvilleHousing
set SoldAsVacant = case 
                  when SoldAsVacant = 'N' then 'No'
			      when SoldAsVacant = 'Y' then 'Yes'
	 		      else SoldAsVacant
			      end
 ------------------------------------------------------------------------------------
--Remove Duplicates

with RowNumCTE as
(
select *,
	ROW_NUMBER() over (
	partition by ParcelID,
	PropertyAddress,
	SaleDate,
	SalePrice,
	LegalReference
	order by
	UniqueID) row_num
from NashvilleHousing 
)
select * from RowNumCTE where row_num > 1

delete  from RowNumCTE where row_num > 1

-----------------------------------------------------------------------------------------------------------

--Delete Unused Columns


alter table NashvilleHousing
drop column SaleDate,PropertyAddress,OwnerAddress,TaxDistrict

-------------------------------------------------------------------------------------------------------------
		   
